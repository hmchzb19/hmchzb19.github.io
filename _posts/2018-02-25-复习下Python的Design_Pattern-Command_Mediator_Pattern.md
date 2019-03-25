#### 看了个韩国人做的Python Design Pattern视频，决定把以前的代码拿出来重新复习下.  

1. command Pattern

        

        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
    
        from abc import ABCMeta,abstractmethod
    
        class Order(metaclass=ABCMeta):
            @abstractmethod
            def execute(self):
                pass
    
        class BuyStockOrder(Order):
            def __init__(self,stock):
                self.stock=stock
    
            def execute(self):
                self.stock.buy()
    
        class SellStockOrder(Order):
            def __init__(self,stock):
                self.stock=stock
    
            def execute(self):
                self.stock.sell()
    
        class StockTrade:
            def buy(self):
                print("You will buy stocks")
            
            def sell(self):
                print("You will sell stocks")
    
    
        class Agent:
            def __init__(self):
                self.__orderQueue=[]
    
            def placeOrder(self,other):
                self.__orderQueue.append(other)
                other.execute()
    
        def test_agent():
            #client
            stock=StockTrade()
            buyStock=BuyStockOrder(stock)
            sellStock=SellStockOrder(stock)
    
            #Inovker
            agent=Agent()
            agent.placeOrder(buyStock)
            agent.placeOrder(sellStock)
    
        test_agent()
    

2. Mediator pattern  

        

        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
        # Author: hezhb
        # Created Time: Sat 24 Feb 2018 03:21:20 PM CST
    
        from abc import ABCMeta,abstractmethod
        import sys
    
        class Colleague(metaclass=ABCMeta):
            
            def __init__(self, mediator, id):
                self._mediator = mediator
                self._id = id
            
            def getId(self):
                return self._id
            
            @abstractmethod
            def send(self, msg):
                pass
            
            @abstractmethod
            def receive(self, msg):
                pass
                
    
        class ConcreteColleague(Colleague):
            def __init__(self, mediator, id):
                super().__init__(mediator, id)
            
            def send(self, msg):
                print("Message {} sent by Colleague {}".format(msg, self._id))
                self._mediator.distribute(self, msg)
            
            def receive(self, msg):
                print("Message {} received by Colleague {}".format(msg, self._id))
            
    
        class Mediator(metaclass=ABCMeta):
            
            @abstractmethod
            def add(self, colleague):
                pass
            
            @abstractmethod
            def distribute(self, sender, msg):
                pass
            
    
        class ConcreteMediator(Mediator):
            def __init__(self):
                Mediator.__init__(self)
                self._colleagues = []
            
            def add(self, colleague):
                self._colleagues.append(colleague)
                
            def distribute(self, sender, msg):
                for coll in self._colleagues:
                    if coll.getId() != sender.getId():
                        coll.receive(msg)
    
        def main():
            mediator = ConcreteMediator()
            
            c1 = ConcreteColleague(mediator, 1)
            c2 = ConcreteColleague(mediator, 2)
            c3 = ConcreteColleague(mediator, 3)
            
            mediator.add(c1)
            mediator.add(c2)
            mediator.add(c3)
            
            c1.send("Good morning")
            
            
        if __name__=="__main__":
            main()
    
        #pyzo里面执行
        In [12]: %run -i /usr/local/src/py/design_pattern/mediator_pattern.py
        Message Good morning sent by Colleague 1
        Message Good morning received by Colleague 2
        Message Good morning received by Colleague 3
    
    