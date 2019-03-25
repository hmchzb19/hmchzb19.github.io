##### 继续昨天的Array　problem:

4.. check two strings are anagrams:  
    eg: "public relations" <--> "crap built on lies"  
    这个anagram_checker 见过无数次了  

        def anagram_checker(s1, s2):
            s1 = s1.replace(' ','').lower()
            s2 = s2.replace(' ','').lower()
            
            return sorted(s1) == sorted(s2)
        
        
        def anagram_checker_2(s1, s2):
            s1 = s1.replace(' ','').lower()
            s2 = s2.replace(' ','').lower()
            
            if len(s1) != len(s2):
                return False 
            
            count = {}
            
            for letter in s1:
                if letter in count:
                    count[letter] +=1 
                else:
                    count[letter] = 1
            
            for letter in s2:
                if letter in count:
                    count[letter] -= 1
                else:
                    count[letter]= 1
            
            for k in count:
                if count[k] != 0:
                    return False 
            
            return True 
        
        def anagram_tester():
            s1="public relations"
            s2="crap built on lies"
            print(anagram_checker(s1,s2))
            print(anagram_checker_2(s1,s2))
            
        anagram_tester() 


5.. given an integer array, output all the unique pairs that sum up to a specific value k  
    eg: pair_sum([1,3,2,2] , 4)  
    get : ((1,3),(2,2))  


        def pair_sum(array, k):
            
            if len(array) < 2:
                return False 
            
            #sets for tracking 
            seen = set()
            output = set()
            
            for num in array:
                target = k - num 
                
                if target not in seen:
                    seen.add(num)
            
                else:
                    output.add((min(num,target), max(num,target)))
                        
            #return len(output)
            return '\n'.join(map(str,list(output)))
        
         
        def pair_sum_test():
            print(pair_sum([1,3,2,2],4))
        
        pair_sum_test()　　


6.. find the missing element:  

'''given an array of non-negative Integers, A second array is formed by shuffle the elements

of the first array and deleting a random element. Given the two arrays, find which

elements is missing in the second array.

'''  
这个XOR(异或)的用法让我想起来以前Ｃ语言不用临时变量(指针)交换两个变量的值：  
c语言里面会这样：  

        a=a^b
        b=a^b
        a=a^b
        
下面是python代码：

        def find_missing_elem(arr1, arr2):
            sortarr1=sorted(arr1)
            sortarr2=sorted(arr2)
            
            for num1, num2 in zip(sortarr1, sortarr2):
                if num1 != num2:
                    return num1 
            
            return sortarr1[-1]
        
        
        import collections
        
        def find_missing_elem2(arr1, arr2):
            d = collections.defaultdict(int)
            
            for num in arr2:
                d[num] += 1
            
            for num in arr1:
                if d[num] == 0:
                    return num
                else:
                    d[num] -= 1
                
        
        #only applies when arr1 and arr2 are small, also they are integers arrays 
        def find_missing_elem3(arr1, arr2):
            return sum(arr1) - sum(arr2)
            
        
        
        #utilize XOR 
        def find_missing_elem4(arr1, arr2):
            
            result = 0
            
            for num in (arr1 + arr2):
                result ^= num 
                #print(result)
            
            return result 
        
        
        
        def find_missing_elem_test():
            print(find_missing_elem4((1,2,3,4),(4,3,2)))
            print(find_missing_elem((1,2,3,4),(4,3,2)))
            print(find_missing_elem2((1,2,3,4),(4,3,2)))
            print(find_missing_elem3((1,2,3,4),(4,3,2)))
        
        find_missing_elem_test()