本来使用sublime text3也可以做python的IDE，但是我也想试试pyzo。  
安装的过程很和平。没有任何报错  

        apt-get install pyzo

但是一启动就报错.  

        pyzo
        Started our command server
        Segmentation fault

查看/var/log/messages就看到这么一行  

        Aug 13 17:48:18 kali kernel: [14934.509890] pyzo[4410]: segfault at 0 ip 00007f8688607561 sp 00007fff914d47b0 error 6 in QtCore.cpython-36m-x86_64-linux-gnu.so[7f86884aa000+161000]

思考了下，决定从这个.so(Shared Object)入手。  
先找找这个文件在哪里。  

        locate QtCore.cpython-36m-x86_64-linux-gnu.so
        /usr/lib/python3/dist-packages/PyQt4/QtCore.cpython-36m-x86_64-linux-gnu.so
        /usr/lib/python3/dist-packages/PyQt5/QtCore.cpython-36m-x86_64-linux-gnu.so

有点意思，应该是pyqt5和pyqt4都提供了这个文件。确认下提供这个文件的包名。  

        root@kali:/usr/local/src/py# dpkg -l |grep pyqt
        ii  python-pyqt5                                      5.11.2+dfsg-1                    amd64        Python 2 bindings for Qt5
        ii  python3-pyqt4                                     4.12.1+dfsg-2                    amd64        Python3 bindings for Qt4
        ii  python3-pyqt5                                     5.11.2+dfsg-1                    amd64        Python 3 bindings for Qt5
        ii  python3-pyqt5.qtmultimedia                        5.11.2+dfsg-1                    amd64        Python 3 bindings for Qt5's Multimedia module
        ii  python3-pyqt5.qtopengl                            5.11.2+dfsg-1                    amd64        Python 3 bindings for Qt5's OpenGL module
        ii  python3-pyqt5.qtsvg                               5.11.2+dfsg-1                    amd64        Python 3 bindings for Qt5's SVG module
        ii  python3-pyqt5.qtwebkit                            5.11.2+dfsg-1                    amd64        Python 3 bindings for Qt5's WebKit module

我决定把python3-pyqt4给卸掉试试运气。  

        apt-get remove python3-pyqt4
        #call pyzo again
        pyzo
        Started our command server
        Traceback (most recent call last):
          File "/usr/lib/python3/dist-packages/qtpy/__init__.py", line 148, in <module>
            from PySide import __version__ as PYSIDE_VERSION # analysis:ignore
        ModuleNotFoundError: No module named 'PySide'
        During handling of the above exception, another exception occurred:
        Traceback (most recent call last):
          File "/usr/bin/pyzo", line 11, in <module>
            load_entry_point('pyzo==4.4.3', 'console_scripts', 'pyzo')()
          File "/usr/lib/python3/dist-packages/pkg_resources/__init__.py", line 480, in load_entry_point
            return get_distribution(dist).load_entry_point(group, name)
          File "/usr/lib/python3/dist-packages/pkg_resources/__init__.py", line 2693, in load_entry_point
            return ep.load()
          File "/usr/lib/python3/dist-packages/pkg_resources/__init__.py", line 2324, in load
            return self.resolve()
          File "/usr/lib/python3/dist-packages/pkg_resources/__init__.py", line 2330, in resolve
            module = __import__(self.module_name, fromlist=['__name__'], level=0)
          File "/usr/share/pyzo/pyzo/__init__.py", line 87, in <module>
            from pyzo.util.qt import QtCore, QtGui, QtWidgets
          File "/usr/share/pyzo/pyzo/util/qt/__init__.py", line 13, in <module>
            from qtpy import *
          File "/usr/lib/python3/dist-packages/qtpy/__init__.py", line 154, in <module>
            raise PythonQtError('No Qt bindings could be found')
        qtpy.PythonQtError: No Qt bindings could be found

我的运气就这么来了， `No module named 'PySide'`. 装上Pyside  

        root@kali:/usr/local/src/py# apt-cache search pyside
        libpyside-dev - Python bindings for Qt 4 (development files)
        libpyside-py3-1.2 - Python3 bindings for Qt 4 (base files)
        libpyside1.2 - Python bindings for Qt 4 (base files)
        libpythonqt-qt5-common-dev - Dynamic Python binding for the Qt framework - development
        libpythonqt-qt5-python2-3 - Dynamic Python binding for the Qt framework - runtime
        libpythonqt-qt5-python2-dev - Dynamic Python binding for the Qt framework - development
        libpythonqt-qt5-python3-3 - Dynamic Python binding for the Qt framework - runtime
        libpythonqt-qt5-python3-dev - Dynamic Python binding for the Qt framework - development
        libpythonqt-qtall-qt5-common-dev - Dynamic Python binding for the Qt framework - development
        libpythonqt-qtall-qt5-python2-3 - Dynamic Python binding for the Qt framework - runtime
        #此处删掉了50行 -for brevity

        root@kali:/usr/local/src/py# apt-get install python3-pyside
        root@kali:/usr/local/src/py# apt-get install python3-pyqt4


然后再运行pyzo.   

        root@kali:~# pyzo
        Started our command server
        Stopped our command server.
        root@kali:~# 
        Pyzo - Python to the people!

        Version info
        Pyzo version: 4.4.3 (source)
        Platform: linux
        Python version: 3.6.6
        Qt version: 4.8.7
        PySide version: 1.2.2

        Pyzo directories
        Pyzo source directory: /usr/share/pyzo/pyzo
        Pyzo userdata directory: /root/.pyzo

        Acknowledgements
        Pyzo is written in Python 3 and uses the Qt widget toolkit. Pyzo uses code and concepts that are inspired by IPython, Pype, and Spyder. Pyzo uses a (modified) subset of the silk icon set, by Mark James (http://www.famfamfam.com/lab/icons/silk/). 

EOF