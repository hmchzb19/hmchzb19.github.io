python的getattr用于normal attribute lookup fails的情况, 例如某attribute不存在
而getattribute会拦截所有的atttribute lookup 操作，不论这个attribute 存在与否  

看代码，当我们search for并不存在p.f的时候，会调用`__getattr__`
```
    class P:
        x = 'a' 
        def __init__(self,n=0):
            self.n = n 

        def __getattr__(self,name):
            return("__geattr__ will be called when normal attribute get failed")
        
        '''
        def __getattribute__(self, name):
            return("__getattribute__ will be called always")
        '''

    def test_():
        p = P()
        print(p.x)
        print(p.n)
        print(p.__dict__)
        #with parameter, equals p.__dict__
        print(vars(p))
        #searching for a attribute which is not exist
        print(p.f)
        print(getattr(p, 'n'))
        print(getattr(p, 'x'))
```
运行结果是  
```
    a
    0
    {'n': 0}
    {'n': 0}
    __geattr__ will be called when normal attribute get failed
    0
    a
```
下面的这个例子，因为有`__getattribute__`的存在，所有的attribute looup操作都会通过该函数　　
```
class B:
    x = 'ba' 
    def __init__(self,n=100):
        self.n = n 

    def __getattr__(self,name):
        return("__geattr__ will be called when normal attribute get failed")
    
    def __getattribute__(self, name):
        return("__getattribute__ will be called always")

def test2_():
    b = B()
    print(b.x)
    print(b.n)
    print(b.__dict__)
    #With parameter, equals to b.__dict__
    print(vars(b))
    #searching for a attribute which is not exist
    print(b.f)
    print(getattr(b, 'n'))
    print(getattr(b, 'x'))
```
运行结果是　　
```
    __getattribute__ will be called always
    __getattribute__ will be called always
    __getattribute__ will be called always
    __getattribute__ will be called always
    __getattribute__ will be called always
    __getattribute__ will be called always
    __getattribute__ will be called always
```
