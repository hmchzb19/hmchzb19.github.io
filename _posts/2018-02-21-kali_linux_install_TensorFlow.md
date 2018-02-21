#### TensorFlow早听说过大名，今天也来装装看 

1.  tensorflow的网站我是打不开的，不知道为什么，需要搬个梯子去看看. 
域名解析也正常，只是无法访问而已。 
        

        ping -c4 tensorflow.org
        PING tensorflow.org (216.239.32.21) 56(84) bytes of data.
    
        --- tensorflow.org ping statistics ---
        4 packets transmitted, 0 received, 100% packet loss, time 3056ms 


2. 好在还有github ->  `https://github.com/tensorflow/tensorflow` 

    首先创建virtual - env. 

        python3.5 -m venv TensorEnv
        cd TensorEnv
        mkdir -p TensorFLow 

    
    下载文件

        wget "https://ci.tensorflow.org/view/tf-nightly/job/tf-nightly-linux/TF_BUILD_IS_OPT=OPT,TF_BUILD_IS_PIP=PIP,TF_BUILD_PYTHON_VERSION=PYTHON3.5,label=cpu-slave/lastSuccessfulBuild/artifact/pip_test/whl/tf_nightly-1.head-cp35-cp35m-linux_x86_64.whl" 

    然后使用pip完成安装:

        pip3 install tf_nightly-1.head-cp35-cp35m-linux_x86_64.whl 

    有如下报错:
        

        error: invalid command 'bdist_wheel'
        
        ----------------------------------------
        Failed building wheel for gast 

    需要升级setuptools.

        pip3 install setuptools --upgrade 

    然后再次安装:

        pip3 install tf_nightly-1.head-cp35-cp35m-linux_x86_64.whl

    完成后来实验下：

        

        >>> import tensorflow as tf
        >>> hello = tf.constant('Hello,
        >>> sess = tf.Session()
        2018-02-21 15:06:08.405164: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA 

    TensorFlow编译的时候没有支持CPU的某些指令

    参看这个：
https://stackoverflow.com/questions/47068709/your-cpu-supports-instructions-that-this-tensorflow-binary-was-not-compiled-to-u



