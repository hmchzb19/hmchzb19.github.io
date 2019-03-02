**我这里大部分代码出自The Python Master一书**  


1. 在python的class里面,允许同名的方法存在。但是后定义的方法会覆盖掉先定义的方法.  
例如  
```
    class Dod1:  
        def method1(self):
            return "first definition"
        
        '''The second definition takes precedence because
        it overwrites the first entry in the namespace 
        dictionary as the class definition is processed
        '''
        def method1(self):
            return "second definition"
        
    def test_():
        
        d1 = Dod1()
        print(d1.method1())

    test_()
```

   test_()该函数会返回second definition  
   又如  
```
    class T:
        
        def test1(self, second:str):
            print("I am test1, and second is ", second)
        
        def test1(self):
            print("I am test1")

    def test_():
        
        t = T()
        a = range(1,5)
        
        try:
            t.test1(a)
        except TypeError as e:
            print(e)
        
        t.test1()
        
        

    test_()
```

   这一段代码的运行结果是
```
    test1() takes 1 positional argument but 2 were given
    I am test1
```


2. 但是可以使用metaclass 避免这个情况，即不允许class里面有同名的method.  
we can write metaclass which detects and prevents this.  
rather than using a regular dictionary as the namespace object used during class construction  
we need a dictionary which raises an error when we try to assign to an  existing key.  
代码如下:  
```
    class OneShotDict(dict):
        
        def __init__(self, existing=None):
            super().__init__()
            if existing is not None:
                for k, v in existing:
                    self[k] = v
            
        def __setitem__(self, key, value):
            if key in self:
                raise KeyError("Cannot assign existing key {!r} " 
                "in {!r}".format(key, type(self).__name__))
        
            super().__setitem__(key, value)
        

    class ProhibitDuplicateMeta(type):
        
        @classmethod
        def __prepare__(mcs, name, bases):
            return OneShotDict()


    if __name__ == "__main__":
        '''Can not define a class with duplicate methods using this
        metaclass''' 
        class Dod2(metaclass=ProhibitDuplicateMeta):
            def method1(self): 
                return "first definition"
        
            def method1(self): 
                return "second definition"
```
但是出错信息不够清晰如下：  
`KeyError: "Cannot assign existing key 'method1' in 'OneShotDict'"`  
改进以后则如下:  
```
    class OneShotClassNamespace(dict):
        
        def __init__(self, name, existing=None):
            super().__init__()
            self._name = name
            if existing is not None:
                for k,v in existing:
                    self[k] = v
        
        def __setitem__(self, key, value):
            if key in self:
                raise TypeError("Can not reassign existing class "
                "attribute {!r} of {!r}".format(key, self._name))
        
            super().__setitem__(key, value)

    class ProhibitDupInClass(type):
        
        @classmethod
        def __prepare__(mcs, name, bases):
            return OneShotClassNamespace(name)

    if __name__ == "__main__":
        '''Can not define a class with duplicate methods using this metaclass'''
        class Dod2(metaclass=ProhibitDupInClass):
            
            def method1(self):
                return "first definition"
            
            def method1(self):
                return "second definition"
```
这时候出错信息如下:  
`TypeError: Can not reassign existing class attribute 'method1' of 'Dod2'`

```
    if __name__ == "__main__":
        '''Can not define a class with duplicate methods using this metaclass'''
        class T2(metaclass=ProhibitDupInClass):
            def test1(self, second:str):
                print("I am test1, and second is ", second)
            
            def test1(self):
                print("I am test1")

    #出错信息
    TypeError: Can not reassign existing class attribute 'test1' of 'T2'
```
