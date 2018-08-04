#### kali_linux安装mpv

1. 不知道卸载了什么，发现smplayer, 和mpv 都消失了.  
决定从头开始把视频播放安装回来。  

	    apt-get update
	    apt-get install mpv


2. 安装完打开mpv没有声音，为什么呢？  
我使用的是xfce4的desktop, 从Application->Multimedia->PulseAudio Volume  Control点进去，怀疑是声音的某个组件丢失了。搞定的方式如下：  

		apt-cache search pavucontrol
		apt-cache search pulseaudio
		apt-get install pavucontrol
		apt-get install pulseaudio


3. 文档在这里  
https://mpv.io/manual/stable/ 


4. 最后，mpv是一款优秀的播放器，最近我在用mpv看The Wire, 软件和电视剧都很不错.  