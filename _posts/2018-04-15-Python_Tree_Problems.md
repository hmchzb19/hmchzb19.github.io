1..

is this Binary Tree a Binary Search Tree.　　
对于Binary Search Tree,如果使用inorder traversal, 则可以得到一个ordered list.  
所以这种方法最简单：　　


            #BST Node class
            class Tree_Node:
                __slots__='value','left','right'
                
                def __init__(self, value, left=None, right=None):
                    self.value = value
                    self.left = left
                    self.right = right
        
            #binary search Tree: inorder traversal will get a sorted list
            def is_bst2(tree):
                tree_values = []
                
                def inorder(tree):
                    if tree != None:
                        inorder(tree.left)
                        tree_values.append(tree.value)
                        inorder(tree.right)
                
                inorder(tree)
                #print(tree_values)
                return sorted(tree_values) == tree_values
        
    
    
    另外一个思路则是使用递归，多使用两个变量，记录当前的key上限和下限，然后比较所有Node是否小于上限并且大于下限。然后左右Node递归。  
    
    
    
        #use recursion
        def is_bst(node, lower_lim=None, upper_lim=None):
            if lower_lim is not None and node.value < lower_lim:
                return False
            if upper_lim is not None and upper_lim < node.value:
                return False
            
            is_left_bst = True
            is_right_bst = True
            
            if node.left is not None:
                is_left_bst = is_bst(node.left, lower_lim, node.value)
            if node.right is not None:
                is_right_bst = is_bst(node.right, node.value, upper_lim)
            
            return is_left_bst and is_right_bst
    
        def test_is_bst():
            root=Tree_Node(10)
            root.left = Tree_Node(5)
            root.right = Tree_Node(15)
            root.right.left = Tree_Node(12)
            root.right.right = Tree_Node(20)
            
            #root.left.right =Tree_Node(1)
            
            print(is_bst2(root))
            print(is_bst(root))
    
        test_is_bst()
    
    

2..  　

print tree level　　
        
        ''' eg: tree below 
                1 
               / \
              2   3
             /     \
            4       5
        
        will be  printed as  
            "1 
             2 3 
             4 5"
        '''
        import collections
        def levelOrderPrint(tree):
            
            if tree == None:
                return 
            nodes = collections.deque([tree])
            
            currentCount =1 
            nextCount = 0 
            
            while len(nodes) != 0:
                currentNode = nodes.popleft()
                currentCount -= 1
                
                print(currentNode.value,end=' ')
                
                if currentNode.left:
                    nodes.append(currentNode.left)
                    nextCount += 1
                
                if currentNode.right:
                    nodes.append(currentNode.right)
                    nextCount += 1
                
                if currentCount == 0:
                    print()
                    currentCount, nextCount = nextCount, currentCount 
        
        
        def test_levelOrderPrint():
            root=Tree_Node(1)
            root.left=Tree_Node(2)
            root.right=Tree_Node(3)
            root.left.left=Tree_Node(4)
            root.right.right=Tree_Node(5)
            
            levelOrderPrint(root)
        
        
        #test_levelOrderPrint()


3..  
lowest common ancestors , assume no duplicates in the binary tree  
找到两个Node 最近的共同祖先。  
这个方法是从head 到两个Node的Path返回一个list,  
然后从list里面开始pop，开始比较，直到发现不同的Node, break,返回最后一个相同的Node .   


        #lower common ancestors , no duplicates in the binary tree
        def lca(root, value1, value2):
            path_to_value1 = path_to_x(root, value1)
            path_to_value2 = path_to_x(root, value2)
            [ print (i.value) for i in path_to_value1]
            [ print (i.value) for i in path_to_value2]
            
            if path_to_value1 is None or path_to_value2 is None:
                return None
                
            lca_to_return = None
            
            
            while len(path_to_value1) !=0 and len(path_to_value2)!=0:
                value1_pop = path_to_value1.pop()
                value2_pop = path_to_value2.pop()
                if value1_pop == value2_pop:
                    lca_to_return = value1_pop
                else:
                    break
            
            return lca_to_return
            
    
        #return a list from node to x
        def path_to_x(root, x):
            
            if root is None:
                return None
            
            if root.value == x:
                return [root]
                
            left_path = path_to_x(root.left, x)
            if left_path is not None:
                left_path.append(root)
                return left_path
            
            right_path = path_to_x(root.right, x)
            if right_path is not None:
                right_path.append(root)
                return right_path
            
            return None
            
    
        def test_lca():
            ''' Tree like this
                             5(head)
                           / \
                         1 4
                       / \ / \
                      3 8 9 2
                     / \
                    6 7
            lca(head, 8, 7) -> return 1
            '''
            
            head = Tree_Node(5)
            head.left = Tree_Node(1)
            head.left.left= Tree_Node(3)
            head.left.right = Tree_Node(8)
            head.left.left.left = Tree_Node(6)
            head.left.left.right =Tree_Node(7)
            head.right= Tree_Node(4)
            head.right.left = Tree_Node(9)
            head.right.right = Tree_Node(2)
            
            ret = lca(head, 8, 7)
            if ret:
                print(ret.value)
            
        test_lca()
    


4.. 
修剪一个BST, 提供一个最小值和一个最大值，返回值在其中的Ｎode,并且仍然保留BST的特性。  




        '''
        Given the root of a binary search Tree and 2
        numbers min and max, trim the tree such that all the numbers in the new tree
        are between min and max (inclusive),The resulting tree should still be a valid
        binary search Tree.
        eg:
                     8
                   / \
                  3 10
                 / \ \
                1 6 14
                   / \ /
                  4 7 13
    
        min=5, max=13
    
        will get resulting BST:
    
                      8
                    / \
                   6 10
                    \ \
                     7 13
        '''
    
    
        def trim_bst(tree,min_value, max_value):
            #use postorder traversal
    
            if tree == None:
                return None
            
            tree.left = trim_bst(tree.left, min_value, max_value)
            tree.right = trim_bst(tree.right, min_value, max_value)
            
            if min_value <= tree.value <= max_value:
                return tree
            
            if tree.value < min_value:
                return tree.right
            
            if tree.value > max_value:
                return tree.left
    
        def inorder_traversal(root):
            
            if root is None:
                return
            else:
                inorder_traversal(root.left)
                print(root.value)
                inorder_traversal(root.right)
                
            
            
        def test_trim_bst():
            root=Tree_Node(8)
            root.left= Tree_Node(3)
            root.left.left = Tree_Node(1)
            root.left.right=Tree_Node(6)
            root.left.right.left = Tree_Node(4)
            root.left.right.right = Tree_Node(7)
            
            root.right=Tree_Node(10)
            root.right.right= Tree_Node(14)
            root.right.right.left = Tree_Node(13)
            
            
            trim_bst(root, 5, 13)
            
            #inorder traversal will get sorted output
            #inorder_traversal(root)
            
            
            levelOrderPrint(root)
            
            
        #test_trim_bst()
    
    