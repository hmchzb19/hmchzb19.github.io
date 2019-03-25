1..

reverse a singly-linked-list , given the head, return the new head.　　

在Ｃ语言里面这个题就是纯粹的指针问题. 　　



        class Node:
            __slots__='element','next'
            
            def __init__(self,element,next=None):
                self.element=element
                self.next=next
            
            def __str__(self):
                return repr(self.element)
    
        #reverse a singly-linked-list , given the head, return the new head .
        def reverse_ll(head):
            current = head
            next = None
            prev = None
            
            while current != None :
                next = current.next
                
                current.next = prev
                
                prev = current
                current = next
            
            return prev
    
        def test_reverse():
            a=Node(1)
            b=Node(2)
            c=Node(3)
            d=Node(4)
            a.next=b
            b.next=c
            c.next=d
            
            #print as 1 2 3 4
            head = a
            while head !=None:
                print(head,end='\t')
                head=head.next
            
            print()
            #reverse will get printed as 4　3　2　1
            newhead = reverse_ll(a)
            
            while newhead != None:
                print(newhead,end ='\t')
                newhead = newhead.next
            
            
        test_reverse()
    


2..  
find nth Node from end   

找出倒数第几个node的值，其实就是两个指针，前后两个指针距离为n-1，当后面指针指向最后一个Ｎode的时候，前面的指针就指向了倒数第n个node.  


        def nth_elem_from_end(head, n):
            left = head
            right = head
            
            for i in range(n-1):
                if right.next is None:
                    return None
                right = right.next
            while right.next is not None:
                right = right.next
                left = left.next
            return left
    
        def test_nth_elem_from_end():
            a=Node(1)
            b=Node(2,a)
            c=Node(3,b)
            d=Node(4,c)
            e=Node(5,d)
            
            #current order : head=e->d->c->b->a
            
            target_node = nth_elem_from_end(e,2)
            print(target_node)
    

