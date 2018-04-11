接上文

7..  
given a string, determine if it is comprised of all unique character:  
eg:  
    'abcde' -> True  
    'abcda' -> False  
这个题，第一反应是将list转换成set, 比较length, 结果就很明白了。  

        def uni_chars(s):
            return len(s) == len(set(s))
        
        def uni_chars_2(s):
            chars = set()
            
            for letter in s:
                if letter in chars:
                    return False 
                else:
                    chars.add(letter)
            
            return True 
        
        print(uni_chars('abcde'))
        print(uni_chars('abcda'))
        print(uni_chars_2('abcde'))
        print(uni_chars_2('abcda'))　　

8..  
compress string:  
eg: aaabbc -> a3b2c1  
eg: AAAaaa -> A3a3  
我记得以前看过某个博文讲压缩的时候讲过这个压缩.  

        def string_compress(s):
            r = ''
            l=len(s)
            
            if l == 0:
                return ''
            if l == 1:
                return s+"1"
                
            cnt = 1
            i = 1
            
            while i < l:
                if s[i] == s[i-1]:
                    cnt += 1
                else:
                    r = r+s[i-1]+str(cnt)
                    cnt = 1
                
                i += 1
            
            r = r+s[i-1]+str(cnt)
        
            return r
        
        print(string_compress('AAB'))　　



9..  
given a string of words, reverse all the words 　
这种题单行脚本就足够了

        def rev_words(s):
            return ' '.join(reversed(s.split()))
        
        def rev_words_2(s):
            return ' '.join(s.split()[::-1])
            
        def rev_words_3(s):
            words=[]
            length = len(s)
            spaces = [' ']
            
            i = 0
            while i < length:
                if s[i] not in spaces:
                    word_start = i
                    while i < length and s[i] not in spaces:
                        i += 1
                    words.append(s[word_start:i])
            
                i += 1
            return ' '.join(reversed(words))
        
        print(rev_words('Hi John, are you ready to go?'))
        print(rev_words_2('Hi John, are you ready to go?'))
        print(rev_words_3('Hi John, are you ready to go?'))  


10..  
Given an array of integers (positive and negative) find the largest continuous sum  
这个题我很喜欢，有点意思，因此放在最后　　


        def large_cont_sum(arr):
            if len(arr)==0:
                return 0 
            
            max_sum = current_sum = arr[0]
            
            for num in arr[1:]:
                current_sum = max(current_sum + num, num)
                max_sum = max(current_sum, max_sum)
                
            return max_sum 
            
        
        print(large_cont_sum([1,2,-1,3,4,10,10,-10,-1]))