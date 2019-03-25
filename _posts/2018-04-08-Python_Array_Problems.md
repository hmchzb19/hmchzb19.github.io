最近看了点视频，用代码记录下思路，今天是Array problem. 

1.. find the most frequently occurred item in an Array　

思路很简单，就是用词典来记录item出现的次数就可以，key 是item, value是item出现的次数。
注意处理有多个item同时有最高的出现次数就可以了。 　

        def most_frequent(given_array):
            most_count = -1 
            max_item = None
        
            count ={}
            
            for item in given_array:
                if item in count:
                    count[item] += 1 
                else:
                    count[item] = 1 
            """
                if count[item] > most_count:
                    max_item = item 
                    most_count = count[item]
            #return (max_item,most_count)
            """
            #if there is more than 1 item 
            max_value = max(count.values())
            
            return [(i,count[i]) for i in count if count[i] == max_value]
            
        
        ll=[1,2,4,5,7,1,2,2,2,2,1,1,1]
        print(most_frequent(ll))　　　

２..  Find two common elements in 2 sorted Arrays　
在两个已经排序的数组里面找到相同的element. 

        def common_elem(A,B):
            p1=0
            p2=0
            result = []     #List for Python and ArrayList for Java
            while p1 < len(A) and p2 < len(B):
                if A[p1] == B[p2]:
                    result.append(A[p1])
                    p1 += 1
                    p2 += 1
                elif A[p1] > B[p2]:
                    p2 += 1
                else:
                    p1 += 1
            return result

        l1=[1,3,4,6,7,9]
        l2=[1,2,4,5,9,10]
        print(common_elem(l1, l2)) 　
         

3..  is One Array a rotation of Another Array

前提条件：no duplicate in Array A AND Array B 


        def is_rotation(A, B):
            if len(A) != len(B):
                return False
        
            key = A[0]
            key_i= -1 
            
            for i in range(len(B)):
                if B[i] == key:
                    key_i = i 
                    break
            if key_i == -1:
                return False
        
            for i in range(len(A)):
                j= (i+key_i) % len(A)
                if A[i] != B[j]:
                    return False
            return True
        
        la1=[1,2,3,4,5,6,7]
        la2=[4,5,6,7,1,2,3]
        
        print(is_rotation(la1, la2))
       　
       　