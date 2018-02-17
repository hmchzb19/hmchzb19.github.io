#### 阴历年后第一篇，发现Kali Linux的apt-get update报错 
        
        Err:1 http://mirrors.ustc.edu.cn/kali kali-rolling InRelease
        The following signatures were invalid: EXPKEYSIG ED444FF07D8D0BF6 Kali Linux Repository <devel@kali.org>

1. 使用apt-key list来查看下，发现有个keyring.gpg 过期了 
        
        apt-key list

        #省略n行
        /etc/apt/trusted.gpg.d/kali-archive-keyring.gpg
        -----------------------------------------------
        pub rsa4096 2012-03-05 [SC] [expired: 2018-02-02]
              44C6 513A 8E4F B3D3 0875 F758 ED44 4FF0 7D8D 0BF6
        uid [ expired] Kali Linux Repository <devel@kali.org>

#### 解决方案如下： 

1. 下载新的文件: 

        wget https://http.kali.org/kali/pool/main/k/kali-archive-keyring/kali-archive-keyring_2018.1_all.deb 

2. 安装下： 

        apt install ./kali-archive-keyring_2018.1_all.deb 

3.  验证安装：新的expires: 是2021年 

        

        apt-key list
    
        #省略
    
        /etc/apt/trusted.gpg.d/kali-archive-keyring.gpg
        -----------------------------------------------
        pub rsa4096 2012-03-05 [SC] [expires: 2021-02-03]
              44C6 513A 8E4F B3D3 0875 F758 ED44 4FF0 7D8D 0BF6
        uid [ unknown] Kali Linux Repository <devel@kali.org>
        sub rsa4096 2012-03-05 [E] [expires: 2021-02-03] 

4. 删掉了一个历史遗留keyring: 

        

        /etc/apt/trusted.gpg.d/sogou-archive-keyring.gpg
        ------------------------------------------------
        pub rsa4096 2014-04-09 [SC]
              6CE3 5A4E BAB6 7609 4476 BE7C D259 B755 5E1D 3C58
        uid [ unknown] Ubuntu Kylin Archive Automatic Signing Key <council@ubuntukylin.com>
        sub rsa4096 2014-04-09 [E]
    
        #以前安装搜狗拼音的时候遗留的产物。
    
        apt-key del "6CE3 5A4E BAB6 7609 4476 BE7C D259 B755 5E1D 3C58"
        OK

