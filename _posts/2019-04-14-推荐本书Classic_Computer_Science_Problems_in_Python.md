最近在看这本书，复习了DFS,BFS,A*，书上的代码很精妙.  
先贴Search的这些算法.  
````
from __future__ import annotations
from typing import (TypeVar, Iterable, Sequence, Generic, List, Callable,
Set, Deque, Dict, Any, Optional)
from typing_extensions import Protocol
from heapq import heappush, heappop

T= TypeVar('T')

def linear_contains(iterable: Iterable[T], key: T) ->bool:
    for item in iterable:
        if item == key:
            return True
    
    return False


C = TypeVar('C', bound='Comparable')

class Comparable(Protocol):
    def __eq__(self, other:Any) -> bool:
        ...
    
    def __lt__(self:C, other:Any) -> bool:
        ...
    
    def __gt__(self:C, other:Any) -> bool:
        return (not self < other) and self != other
    
    def __le__(self:C, other:Any) -> bool:
        return self < other or self == other
    
    def __ge__(self:C , other:C) ->bool:
        return not self < other
    
    

def binary_contains(sequence: Sequence[C], key:C) -> bool:
    low: int = 0
    high: int = len(sequence) - 1
    while low<= high:
        middle = (low+high)//2
        if sequence[middle] < key:
            low = middle + 1
        elif sequence[middle] > key:
            high = middle -1 
        else:
            return True
        
    return False

def main():
    print(linear_contains([1,5,15,15,15,20], 5))
    print(binary_contains(["a", "d", "e", "f", "z"], "f"))
    print(binary_contains(["John", "mark", "ronald", "sarah"], "sheila"))





class Stack(Generic[T]):
    def __init__(self) ->None:
        self._container: List[T] = []
    
    @property
    def empty(self) ->bool:
        return not self._container 
    
    def push(self, item:T)->None:
        self._container.append(item)
    
    def pop(self) ->T:
        return self._container.pop()
    
    def __repr__(self) ->str:
        return repr(self._container)


class Node(Generic[T]):
    def __init__(self, state:T, parent: Optional[Node], cost: float=0.0,
        heuristic:float=0.0) ->None:
            self.state: T = state
            self.parent: Optional[Node] = parent
            self.cost: float = cost
            self.heuristic: float = heuristic
            
    def __lt__(self, other: Node) ->bool:
        return (self.cost + self.heuristic) < (other.cost + other.heuristic)

def dfs(initial: T, goal_test: Callable[[T], bool], successors:Callable[[T],List[T]]) ->Optional[Node[T]]:
    #frontier is where we've yet to go
    frontier: Stack[Node[T]]= Stack()
    frontier.push(Node(initial, None))
    
    #explored is where we've been
    explored: set[T] = {initial}
    
    #keep going while where is more to explore
    while not frontier.empty:
        current_node: Node[T] = frontier.pop()
        current_state: T = current_node.state
        
        #if we found the goal, we 're done
        if goal_test(current_state):
            return current_node
        
        #check where we can go next and haven't explored
        for child in successors(current_state):
            if child in explored:   #skip children we already explored
                continue
            
            explored.add(child)
            frontier.push(Node(child, current_node))
    return None

def node_to_path(node: Node[T]) ->List[T]:
    path: List[T] = [node.state]
    #work backwards from end to front
    while node.parent is not None:
        node = node.parent
        path.append(node.state)
    path.reverse()
    return path


class Queue(Generic[T]):
    def __init__(self)->None:
        self._container: Deque[T] = Deque()
    
    @property
    def empty(self)->bool:
        return  not self._container
    
    def push(self, item:T) ->None:
        self._container.append(item)
    
    def pop(self) ->T:
        return self._container.popleft()
    
    def __repr__(self) ->str:
        return repr(self._container)
    
def bfs(initial: T, goal_test: Callable[[T], bool], successors:Callable[[T], List[T]]) ->Optional[Node[T]]:
    #frontier is where we've yet to go
    frontier: Queue[Node[T]] = Queue()
    frontier.push(Node(initial, None))
    
    #explored is where we've been 
    explored: Set[T] = {initial}
    
    #keep going while there is more to explore
    while not frontier.empty:
        current_node: Node[T] = frontier.pop()
        current_state: T = current_node.state
        
        #if we found the goal, we 're done
        if goal_test(current_state):
            return current_node
        
        #check where we can go next and haven't explored
        for child in successors(current_state):
            if child in explored:   #skip children already explored
                continue
            explored.add(child)
            frontier.push(Node(child, current_node))
        
    return None


class PriorityQueue(Generic[T]):
    def __init__(self) ->None:
        self._container: List[T] = []
    
    @property
    def empty(self) ->bool:
        return not self._container 
    
    def push(self, item: T) ->None:
        heappush(self._container, item)
    
    def pop(self)->T:
        return heappop(self._container)
    
    def __repr__(self) ->str:
        return repr(self._container)
    
    
def astar(initial: T, goal_test: Callable[[T], bool], successors:Callable[[T], List[T]], heuristic:Callable[[T], float]) ->Optional[Node[T]]:
    #frontier is where we've yet to go
    frontier: PriorityQueue[Node[T]] = PriorityQueue()
    frontier.push(Node(initial, None, 0.0, heuristic(initial)))
    
    #explored is where we've been
    explored: Dict[T, float] = {initial: 0.0}
    
    #keep going while there is more to explore
    while not frontier.empty:
        current_node: Node[T] = frontier.pop()
        current_state: T = current_node.state
        #if we found the goal, we're done
        if goal_test(current_state):
            return current_node
        
        #check where we can go next and haven't explored
        for child in successors(current_state):
            new_cost: float = current_node.cost + 1
            #1 assumes a grid,need a cost function for more sophisticated apps
            if child not in explored or explored[child] > new_cost:
                explored[child] = new_cost
                frontier.push(Node(child, current_node, new_cost, heuristic(child)))
                
    #went through everything and never found goal
    return None
````
接下来是一个missionary问题的解法，３个牧师和３个食人族要到河对岸去，只有一条船，船最多载２个人
任何时候当食人族的数目多于牧师，食人族就会吃掉牧师.  
````
from __future__ import annotations
from typing import List, Optional
from generic_search import bfs, Node, node_to_path

MAX_NUM: int = 3

class MCState:
    
    def __init__(self, missionaries: int, cannibals: int, boat: bool)->None:
        #west bank missionaries 
        self.wm: int = missionaries
        #west bank cannibals
        self.wc: int = cannibals
        #east bank missionaries
        self.em: int = MAX_NUM - missionaries
        #eat bank cannibals
        self.ec: int = MAX_NUM - cannibals
        self.boat: bool = boat
    
    def __str__(self) ->str:
        return ("On the west bank there are {} missionaries and {} cannibals"
        "\nonthe east bank there are {} missionaries and "
        "{} cannibals.\nThe boat is on the {} bank.".format(
            self.wm, self.wc, self.em, self.ec, ("west" if self.boat else "east")))
        
    
    def goal_test(self) -> bool:
        return self.is_legal and self.em == MAX_NUM and self.ec == MAX_NUM
    
    
    @property
    def is_legal(self) ->bool:
        if self.wm < self.wc and self.wm > 0:
            return False
        if self.em < self.ec and self.em > 0:
            return False
        return True
    
    def successors(self) ->List[MCState]:
        sucs: List[MCState] = []
        
        if self.boat:   #boat on west bank
            if self.wm > 1:
                sucs.append(MCState(self.wm -2, self.wc, not self.boat))
            if self.wm > 0:
                sucs.append(MCState(self.wm -1, self.wc, not self.boat))
            if self.wc > 1:
                sucs.append(MCState(self.wm, self.wc -2, not self.boat))
            if self.wc > 0:
                sucs.append(MCState(self.wm, self.wc -1, not self.boat))
            if (self.wc >0 ) and (self.wm > 0):
                sucs.append(MCState(self.wm -1, self.wc -1, not self.boat))
            
        else:
            #Boat on east side
            if self.em > 1:
                sucs.append(MCState(self.wm +2, self.wc, not self.boat))
            if self.em > 0:
                sucs.append(MCState(self.wm +1, self.wc, not self.boat))
            if self.ec > 1:
                sucs.append(MCState(self.wm, self.wc +2, not self.boat))
            if self.ec > 0:
                sucs.append(MCState(self.wm, self.wc +1, not self.boat))
            if (self.ec > 0) and (self.em > 0):
                sucs.append(MCState(self.wm + 1, self.wc + 1, not self.boat))
        
        return [ x for x in sucs if x.is_legal]


def display_solution(path: List[MCState]):
    if len(path) == 0:
        return
    
    old_state: MCState = path[0]
    print(old_state)
    for current_state in path[1:]:
        if current_state.boat:
            print("{} missionaries and {} cannibals moved from the east "
            " bank to the west bank.\n".format(old_state.em - current_state.em, old_state.ec - current_state.ec))
        else:
            print("{} missionaries and {} cannibals moved from the west "
            "bank to the east bank.\n".format(old_state.wm - current_state.wm, old_state.wc - current_state.wc))
            
        print(current_state)
        old_state = current_state
    
    

def main():
    
    start: MCState = MCState(MAX_NUM, MAX_NUM, True)
    solution: Optional[Node[MCState]] = bfs(start, MCState.goal_test,
        MCState.successors)
    if solution is None:
        print("No solution found!")
    else:
        path: List[MCState] = node_to_path(solution)
        display_solution(path)

if __name__ == "__main__":
    main()

#执行结果如下
./missionaries.py
On the west bank there are 3 missionaries and 3 cannibals
onthe east bank there are 0 missionaries and 0 cannibals.
The boat is on the west bank.
0 missionaries and 2 cannibals moved from the west bank to the east bank.

On the west bank there are 3 missionaries and 1 cannibals
onthe east bank there are 0 missionaries and 2 cannibals.
The boat is on the east bank.
0 missionaries and 1 cannibals moved from the east bank to the west bank.

On the west bank there are 3 missionaries and 2 cannibals
onthe east bank there are 0 missionaries and 1 cannibals.
The boat is on the west bank.
0 missionaries and 2 cannibals moved from the west bank to the east bank.

On the west bank there are 3 missionaries and 0 cannibals
onthe east bank there are 0 missionaries and 3 cannibals.
The boat is on the east bank.
0 missionaries and 1 cannibals moved from the east bank to the west bank.

On the west bank there are 3 missionaries and 1 cannibals
onthe east bank there are 0 missionaries and 2 cannibals.
The boat is on the west bank.
2 missionaries and 0 cannibals moved from the west bank to the east bank.

On the west bank there are 1 missionaries and 1 cannibals
onthe east bank there are 2 missionaries and 2 cannibals.
The boat is on the east bank.
1 missionaries and 1 cannibals moved from the east bank to the west bank.

On the west bank there are 2 missionaries and 2 cannibals
onthe east bank there are 1 missionaries and 1 cannibals.
The boat is on the west bank.
2 missionaries and 0 cannibals moved from the west bank to the east bank.

On the west bank there are 0 missionaries and 2 cannibals
onthe east bank there are 3 missionaries and 1 cannibals.
The boat is on the east bank.
0 missionaries and 1 cannibals moved from the east bank to the west bank.

On the west bank there are 0 missionaries and 3 cannibals
onthe east bank there are 3 missionaries and 0 cannibals.
The boat is on the west bank.
0 missionaries and 2 cannibals moved from the west bank to the east bank.

On the west bank there are 0 missionaries and 1 cannibals
onthe east bank there are 3 missionaries and 2 cannibals.
The boat is on the east bank.
1 missionaries and 0 cannibals moved from the east bank to the west bank.

On the west bank there are 1 missionaries and 1 cannibals
onthe east bank there are 2 missionaries and 2 cannibals.
The boat is on the west bank.
1 missionaries and 1 cannibals moved from the west bank to the east bank.

On the west bank there are 0 missionaries and 0 cannibals
onthe east bank there are 3 missionaries and 3 cannibals.
The boat is on the east bank. 
````
