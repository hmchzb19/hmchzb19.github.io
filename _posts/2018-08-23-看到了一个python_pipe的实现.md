这个实现的代码在这里:
[Pipes in Python](http://sulami.github.io/posts/pipes-in-python/)
代码我在这里也照样给抄过来了

        def pype(x, *fs):
            """
            Pipe function. Takes an initial value and any number of functions/methods.
            Methods as strings. Additional args are supported for functions & methods
            by suppling a step as a tuple/list with function/method as the first
            element and the args as the rest. The pipe input is used as the last
            argument in this case. Currently no kwargs.
            """
            while fs:
                f = fs[0]
                args = []
                if isinstance(f, (list, tuple)):
                    args = list(f[1:])
                    f = f[0]
                if isinstance(f, str):
                    if f.startswith('.'):
                        x = getattr(x, f[1:])(*args)
                    else:
                        x = x[f]
                elif isinstance(f, int):
                    x = x[f]
                else:
                    x = f(*args + [x])
                fs = fs[1:]
            return x


我比较感兴趣的是这一句代码：  
`x = f(*args + [x])`，　所以我将这个代码给修改了下,  
`x = f(*args, x)`, 得到的结果完全一样。不知道为什么他会那么写。搞不懂。  
如果写成 `x = f(*args, [x])` ,得到的结果也很有趣。　  
下面就是比较:  

        def pype_2(x, *fs):
            """
            Pipe function. Takes an initial value and any number of functions/methods.
            Methods as strings. Additional args are supported for functions & methods
            by suppling a step as a tuple/list with function/method as the first
            element and the args as the rest. The pipe input is used as the last
            argument in this case. Currently no kwargs.
            """
            while fs:
                f = fs[0]
                args = []
                if isinstance(f, (list, tuple)):
                    args = list(f[1:])
                    f = f[0]
                if isinstance(f, str):
                    if f.startswith('.'):
                        x = getattr(x, f[1:])(*args)
                    else:
                        x = x[f]
                elif isinstance(f, int):
                    x = x[f]
                else:
                    #difference lies here
                    x = f(*args, x)
                fs = fs[1:]
            return x


        def pype_3(x, *fs):
            """
            Pipe function. Takes an initial value and any number of functions/methods.
            Methods as strings. Additional args are supported for functions & methods
            by suppling a step as a tuple/list with function/method as the first
            element and the args as the rest. The pipe input is used as the last
            argument in this case. Currently no kwargs.
            """
            while fs:
                f = fs[0]
                args = []
                if isinstance(f, (list, tuple)):
                    args = list(f[1:])
                    f = f[0]
                if isinstance(f, str):
                    if f.startswith('.'):
                        x = getattr(x, f[1:])(*args)
                    else:
                        x = x[f]
                elif isinstance(f, int):
                    x = x[f]
                else:
                    #difference lies here
                    x = f(*args, [x])
                fs = fs[1:]
            return x

        def add_suffix(number, s):
            return '{} is {} cool!'.format(
                s,
                ' '.join('very' for _ in range(number))
            )

        print(pype(
            ' abc: {} ',
            '.strip',
            ('.format', 3),
            (add_suffix, 2),
            '.upper',
        ))

        print(pype_2(
            ' abc: {} ',
            '.strip',
            ('.format', 3),
            (add_suffix, 2),
            '.upper',
        ))

        print(pype_3(
            ' abc: {} ',
            '.strip',
            ('.format', 3),
            (add_suffix, 2),
            '.upper',
        ))

结果如下：非常有意思  

        In [6]: %run -i /usr/local/src/py/pype.py
        ABC: 3 IS VERY VERY COOL!
        ABC: 3 IS VERY VERY COOL!
        ['ABC: 3'] IS VERY VERY COOL!
