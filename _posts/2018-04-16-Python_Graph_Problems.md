这一篇做这系列文章的结束，Graph是我感觉很难的数据结构，实现数据结构，用数据结构再来解决问题。　　

前面Tree那一篇里面其实也缺少很多复杂的数据结构，例如AVL, RBTree,Skip-List, 这些复杂的数据结构实现复杂.到Graph我记得原来看DAG想死的心都有，现在仍然是觉得这些数据结构真他娘的复杂。死记硬背可以，让我自己去写个实现，用数据结构和算法解决问题真难。　　

图这里其实代码就只有DFS和BFS, DFS是用stack, BFS用queue.当然实际代码里面都用了list. 求2个Ｖertex之间的最短路径用BFS. 　　


        def dfs(graph, start):
            visited = set()
            stack = [start]
            
            while stack:
                vertex = stack.pop()
                
                if vertex not in visited:
                    visited.add(vertex)
                    stack.extend(graph[vertex] - visited)
            
            return visited 
        
        #recursion dfs 
        def rec_dfs(graph, start, visited = None):
            if visited == None:
                visited = set()
            visited.add(start)
            
            for next in graph[start] - visited:
                rec_dfs(graph, next, visited)
            
            return visited 
            
        
        def dfs_paths(graph, start, dest):
            stack = [(start, [start])]
            
            while stack:
                (vertex, path ) = stack.pop()
                for next in graph[vertex] - set(path):
                    if next == dest:
                        yield path + [next]
                    else:
                        stack.append((next, path + [next]))
                        
                
        def test_dfs():
            
            graph = {'A': set(['B', 'C']),
                     'B': set(['A', 'D', 'E']),
                     'C': set(['A','F']),
                     'D': set(['B']),
                     'E': set(['B', 'F']),
                     'F': set(['C','E'])
                     }
            
            print(dfs(graph, 'A'))
            print(rec_dfs(graph, 'A'))
            print(list(dfs_paths(graph, 'A', 'F')))
            
        
        test_dfs()
        
        
        def bfs(graph, start):
            visited = set()
            stack = [start]
            
            while stack:
                vertex = stack.pop(0)
                
                if vertex not in visited:
                    visited.add(vertex)
                    stack.extend(graph[vertex] - visited)
            
            return visited 
            
        
        def bfs_paths(graph, start, dest):
            stack = [(start, [start])]
            
            while stack:
                (vertex, path ) = stack.pop(0)
                for next in graph[vertex] - set(path):
                    if next == dest:
                        yield path + [next]
                    else:
                        stack.append((next, path + [next]))
                        
                
        
        def shortest_path(graph, start, dest):
            
            try:
                return next(bfs_paths(graph, start, dest))
            except StopIteration:
                return None
                
        
        def test_bfs():
            
            graph = {'A': set(['B', 'C']),
                     'B': set(['A', 'D', 'E']),
                     'C': set(['A','F']),
                     'D': set(['B']),
                     'E': set(['B', 'F']),
                     'F': set(['C','E'])
                     }
            
            print(bfs(graph, 'A'))
            
            print(list(bfs_paths(graph, 'A', 'F')))
            
            print(shortest_path(graph, 'A','F'))
        
        test_bfs()