我的笔记本上有多个python3的版本, 3.5, 3.6, 3.7  都有，我有时候想要在不同的python版本间切换。后来得知有两种办法，第一种方法.  


1. 使用`python[VERION] -m IPython`的办法来调用ipython  
```
ython3.6 -m IPythons
Python 3.6.6 (default, Jun 27 2018, 14:44:17)
Type "copyright", "credits" or "license" for more information.

IPython 5.5.0 -- An enhanced Interactive Python.
? -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help -> Python's own help system.
object? -> Details about 'object', use 'object??' for extra details.

In [1]: exit
root@kali:/usr/local/src/py/test_examples# python3.7 -m IPython
Python 3.7.2+ (default, Feb 2 2019, 14:31:48)
Type "copyright", "credits" or "license" for more information.

IPython 5.5.0 -- An enhanced Interactive Python.
? -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help -> Python's own help system.
object? -> Details about 'object', use 'object??' for extra details. 
```


2. ipython 其实是个shell 脚本。我有多个ipython版本，打开看看内容，就是个简单的bash脚本　　
把VERSION这个变量修改下，就能Invoke不同的python版本.  
`3.7就是python3.7`
`3.6就是python3.6`

```
    #! /bin/sh

    VERSION="3.7"

    if [ ! -f /usr/bin/python$VERSION ]
    then
        echo "Please install the python$VERSION package." >&2
        exit 1
    else
        exec python$VERSION -c "import sys; sys.argv[0] = '/usr/bin/ipython$VERSION'; from IPython.terminal.ipapp import launch_new_instance; launch_new_instance()" "$@"
    fi

```