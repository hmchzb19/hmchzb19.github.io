开始玩tensorflow, Kali linux的源暂时没有tensorflow.
`apt-cache search tensorflow`看到有`python3-keras-preprocessing`，只能用`pip`来安装.

我以前安装过tensorflow１.x,现在就算是升级吧.
安装命令:
`pip3 install tensorflow==2.1.0`

碰到的问题: 
1. `python3-wrapt`这个包版本太低，需要卸载掉使用`apt-get`安装的，使用`pip3`重新安装  
`apt-get remove python3-wrapt`
2. grpcio的版本低，需要使用`pip3`升级.  
`ERROR: tensorboard 2.1.1 has requirement grpcio>=1.24.3, but you'll have grpcio 1.23.0 which is incompatible.`

3. `pip3 install tensorflow==2.1.0`安装出来的tensorflow不支持gpu和很多cpu的指令集。不过我的GPU是板载的,只能安装cpu版本的tensorflow. 参考了下这里[custom builds of tensorflow](https://github.com/lakshayg/tensorflow-build),我打算在venv里面重新装个支持cpu指令集的tensorlfow.
`cat /proc/cpuinfo |grep -E "fma|avx|avx2|sse4.2|sse4.1"` 使用这个命令查了下cpu支持的指令集.
