为了能安装一个支持CPU指令集的tensorflow，就是为了解决这个问题`Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE3 SSE4.1 SSE4.2 AVX AVX2 FMA`
我在python3.5的venv里面重新安装了个tensorflow,使用别人做好的[binary文件](https://github.com/lakshayg/tensorflow-build-archived/releases/download/tf-2.1.0-py35-ubuntu1604/tensorflow-2.1.0-cp35-cp35m-linux_x86_64.whl)

步骤如下：　
1. #create venv  
`python3.5 -m venv TensorEnv`

2. #install tensorflow in venv  
`pip3.5 install https://github.com/lakshayg/tensorflow-build-archived/releases/download/tf-2.1.0-py35-ubuntu1604/tensorflow-2.1.0-cp35-cp35m-linux_x86_64.whl`

3. 我的操作系统里面的Python已经升级到了3.8. 感觉kali真坑，`apt-get update`升级python的版本后，我都要自己收拾半天，不是输入法出错，就是`pyzo`不能用了，在venv里面使用python3.5还有tkinter的问题，matplotlib需要tkinter，我看了很多解决方案都是`apt-get python3-tk`,但是我的系统里面已经有了`python3-tk`.  
    ````
    dpkg --get-selections | grep python3-tk
    python3-tk:amd64                install

    #最后我决定给matplotlib换个backend.在evnv里面安装上PyQt5.
    pip3.5 install PyQt5

    #在所有的文件import matplotlib后，换掉backend.
    #matplotlib 使用'Qt5Agg'
    import matplotlib as mpl
    mpl.use('Qt5Agg')

    ````

4. 至此venv的tensorflow环境算是能跑起来了.
