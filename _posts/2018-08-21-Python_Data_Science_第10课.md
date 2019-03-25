#### Bayesian Methods to create Anti-spammer
We can construct P(Spam | Word) for every (meaningful) word we encounter
during training.  
Then multiply these together when analyzing a new mail to get the probability of it being spam.  
Assumes the presence of different words are independent of each other - one reason this is called "Naive Bayes"  

理论就是:  不考虑词和词之间的关系，简单的将每个词贡献的'spam‘值算出来，最后根据所有的这些词贡献出的'spam'值来分析新的邮件。  

下面则是代码  
首先是使用pandas读入数据，然后使用scikit-learn 来build 一个spam classifier,   最后使用这个spam classifier 来predict两个字符串到底应该归类spam 或者ham.  


        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
        # Author: hezhb
        # Created Time: Tue 01 May 2018 11:49:35 AM CST

        import os
        import io
        import numpy as np
        from pandas import DataFrame
        from sklearn.feature_extraction.text import CountVectorizer
        from sklearn.naive_bayes import MultinomialNB

        def readFiles(path):
            for root, dirnames, filenames in os.walk(path):
                for filename in filenames:
                    path = os.path.join(root, filename)
            
                    inBody = False
                    lines = []
                    
                    f = io.open(path, 'r', encoding='latin1')
                    for line in f:
                        if inBody:
                            lines.append(line)
                        elif line == '\n':
                            inBody = True
                        
                    f.close()
                    message = '\n'.join(lines)
                    yield path, message
            

        def dataFrameFromDirectory(path, classification):
            rows = []
            index = []
                                
            for filename, message in readFiles(path):
                rows.append({'message':message, 'class':classification})
                index.append(filename)
            
            return DataFrame(rows, index=index)


        PATH='./hands-on/emails/'


        data = DataFrame({'message':[], 'class':[]})
        data = data.append(dataFrameFromDirectory(PATH+'spam', 'spam'))
        data = data.append(dataFrameFromDirectory(PATH+'ham', 'ham'))
        #print(data.head())

        """
        Now we will use CountVectorizer to split up each message into its list of words
        and throw that into a MultinomialNB classifier, call fit() and we've got
        a trained spam filter ready to go.
        """

        vectorizer = CountVectorizer(encoding='latin1')
        counts = vectorizer.fit_transform(data['message'].values)
        classifier = MultinomialNB()
        targets = data['class'].values
        classifier.fit(counts, targets)

        #Now can try this classifier out
        examples = ['Free viagra Now', 'Hi Bob, how about a game of golf tommorrow.']
        example_counts = vectorizer.transform(examples)
        predictions = classifier.predict(example_counts)
        print(predictions)

