我使用了这里的解决方案:  
[Sublime Text 3 Input Method(Fcitx) Fix [Ubuntu(Debian)]](https://github.com/lyfeyaj/sublime-text-imfix)
实践证明它同样适用kali_linux.  

        cd /opt
        git clone https://github.com/lyfeyaj/sublime-text-imfix.git
        cd sublime-text-imfix/
        #这是个脚本文件，可以自己看看
        vim sublime-imfix
        ./sublime-imfix
        #重新login就能在sublime text 3中使用中文输入了

