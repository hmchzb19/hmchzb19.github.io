##### 最近在看一个Sequence Learning 的视频，然后研究了别人的代码，因为我的"typo" 导致代码有点”歪“，但是揭露了一个有趣的现象. 

1. 这个代码是一个category_predictor,需要用到下面的这个库. `sklearn`  
在kali linux上安装则需要执行这个命令： 

        pip3 install sklearn

2. 代码如下: 

        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
        # Author: hezhb
        # Created Time: Mon 26 Feb 2018 04:27:53 PM CST
        
        from sklearn.datasets import fetch_20newsgroups 
        from sklearn.naive_bayes import MultinomialNB
        from sklearn.feature_extraction.text import TfidfTransformer
        from sklearn.feature_extraction.text import CountVectorizer 
        
        #Define the category map 
        category_map = {
            "talk.politics.misc" : "Politics", 
            "rec.autos" : "Autos",
            "rec.sport.hockey": "Hockey",
            "sci.electronics": "Electronics",
            "sci.med" : "Medicine" }
        
        #get the training dataset 
        training_data = fetch_20newsgroups(subset="train",
            categories=category_map.keys(), shuffle=True, random_state=5 )
            
        
        #Build a count vectorizer and extract term counts 
        count_vectorizer = CountVectorizer()
        train_tc = count_vectorizer.fit_transform(training_data.data)
        print("\nDimensions of training data:", train_tc.shape)
        
        #create the tf-idf transformer
        tfidf= TfidfTransformer()
        train_tfidf=tfidf.fit_transform(train_tc)
        
        #Define test data 
        input_data=[
            "You need to be careful with cars when you are driving on slippery roads",
            "A lot of devices can be operated wirelessly",
            "Players need to be careful when they are close to goal posts",
            "Political debates help us understand the perspectives of both sides",
            "Political debates help us understand the perspectives of both slides",
            "Political debates help us understand the perspective of both sides",
            ]
        
        #Train a Multinomial Naive Bayes classifier 
        classifier = MultinomialNB().fit(train_tfidf, training_data.target)
        
        #Transform input data using count  vectorizer 
        input_tc = count_vectorizer.transform(input_data)
        
        #Transform vectorized data using tfidf transformer 
        input_tfidf = tfidf.transform(input_tc)
        
        #predict the output categories
        predictions=classifier.predict(input_tfidf)
        
        #print the outputs 
        for sent, category in zip(input_data, predictions):
            print("\nInput:", sent , "\nPredicted category:", \
                category_map[training_data.target_names[category]])
            

    执行结果很有意思： 

        In [15]: %cd /usr/local/src/py/py_nlp/usr/local/src/py/py_nlp

        In [16]: %run -i /usr/local/src/py/py_nlp/category_predictor.py
        
        Dimensions of training data: (2844, 40321)
        
        Input: You need to be careful with cars when you are driving on slippery roads 
        Predicted category: Autos
        
        Input: A lot of devices can be operated wirelessly 
        Predicted category: Electronics
        
        Input: Players need to be careful when they are close to goal posts 
        Predicted category: Hockey
        
        Input: Political debates help us understand the perspectives of both sides 
        Predicted category: Politics
        
        Input: Political debates help us understand the perspectives of both slides 
        Predicted category: Medicine
        
        Input: Political debates help us understand the perspective of both sides 
        Predicted category: Medicine


    后面三句完全是一个letter的差距，category就会变.
    sides:方面，
    slides: 这个词有滑落的意思，为什么变成slides就会被predict成 Medicine category.
    最后一句:
    perspectives：复数变单数，也会影响这个句子的归类。Category 类别为Medicine.    

3. 结果：
这是个有趣的现象，但是我现在不知道导致这个现象的原因是什么。