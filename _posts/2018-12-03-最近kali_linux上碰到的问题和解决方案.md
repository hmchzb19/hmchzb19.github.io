1. apt-get update 一直在报错，改用中科大源以后恢复正常。  

        E: Failed to fetch http://mirrors.neusoft.edu.cn/kali/dists/kali-rolling/main/Contents-amd64.gz File has unexpected size (36101836 != 36098798). Mirror sync in progress? [IP: 219.216.128.25 80]
           Hashes of expected file:
            + Filesize:36098798 [weak]
            + SHA256:770615b7786413d184dc2bdacbdb1b7195cc110b2945c4d774b0b7af53d68078
            + SHA1:689212b1e44c904ad68d0146e22628a39332670c [weak]
            + MD5Sum:5df9638a8305cf33fbfdc579f1184560 [weak]
           Release file created at: Mon, 03 Dec 2018 00:04:09 +0000
        E: Failed to fetch http://mirrors.neusoft.edu.cn/kali/dists/kali-rolling/main/Contents-i386.gz
        E: Some index files failed to download. They have been ignored, or old ones used instead.

        vim /etc/apt/sources.list

        #中科大kali source
        deb http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free
        #deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free


2. Firefox-esr 从52就一直无法观看微博和土豆上的视频。Ctrl + Shift +J 查看console   能看到报错.  

`your system may not have the required video codecs for: video/mp4; codecs="mp4v.20.8"`

缺少相应的mp4的codec. 这其实是Firefox的一个bug，升级到60esr以后就可以正常观看视频了。  
而且使用Firefox感觉比以前速度有提升。 