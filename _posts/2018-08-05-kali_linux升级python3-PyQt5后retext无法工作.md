#### retext是编辑markdown文件的工具，最近升级了python3-PyQt5后，发现retext无法正常工作了

1. 报错如下

        apt-get install python3-pyqt5
        apt install retext

        root@kali:~# retext
        Traceback (most recent call last):
          File "/usr/bin/retext", line 26, in <module>
            from ReText import datadirs, settings, globalSettings, app_version
          File "/usr/share/retext/ReText/__init__.py", line 24, in <module>
            from PyQt5.QtCore import QByteArray, QLocale, QSettings, QStandardPaths
        RuntimeError: the sip module implements API v12.0 to v12.3 but the PyQt5.QtCore module requires API v12.4
        root@kali:~# sip -V
        4.19.12



2. 根据报错能看出，python3-sip的和PyQt5的版本不匹配是根本原因，幸好除了retext, 我还有
sublime , 在sublime text3 里面安装两个插件，使得sublime text3可以支持markdown.  

        Ctrl + Shift + P
        安装2个插件
        Markdown Editing
        Markdown Preview

