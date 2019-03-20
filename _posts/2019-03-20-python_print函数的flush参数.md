今天发现了一个奇怪的现象，直接上代码  
```
import sys
from colorama import Fore, Back, Style 

def print_example():
    print(Fore.RED+ "1. This is the first sentence")
    text=' Blowing in the wind'
    print(Fore.WHITE + '2. text =', end='')
    #print('2. text= ', end='')
    print(text,file=sys.stderr)
    print(Fore.CYAN + "3. Do they printed in right order")

print_example()
```
打印出来的结果并不是我设想的顺序  
```
1. This is the first sentence
 Blowing in the wind
2. text =3. Do they printed in right order
```
为什么stderr和stdout会打印到一起呢? 我的猜想也许跟stdout, stderr的缓冲机制有关
后来发现python 3.3 后print可以加flush参数, 这样就能得到我想要的打印次序
```
import sys
from colorama import Fore, Back, Style 

def print_example():
    print(Fore.RED+ "1. This is the first sentence")
    text=' Blowing in the wind'
    print(Fore.WHITE + '2. text =', end='',flush=True)
    #print('2. text= ', end='')
    print(text,file=sys.stderr)
    print(Fore.CYAN + "3. Do they printed in right order")

print_example()

打印出来如下
1. This is the first sentence
2. text = Blowing in the wind
3. Do they printed in right order

```
最后附上参考资料如何使用python打印彩色  
[Print Colors in Python terminal](https://www.geeksforgeeks.org/print-colors-python-terminal/)
