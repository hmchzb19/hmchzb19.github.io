最近到了个新环境，给了我无线网络的ssid,用户名和密码。设置无线网络费了点功夫。记下来

1. 我想知道这个网络到底是什么加密方式。用这个命令  
````
iwlist wlan0 scanning
在查看对应的SSID的返回结果中我看到了这些
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : 802.1x
````
2. 根据802.1x和CCMP这两个东西让我来猜怎么配置， 
首先停掉了networking  
然后启用Network-Manager.  
毕竟图形配置比配置/etc/network/interfaces这个文件要简单吧  
````


    service networking stop
    修改配置文件/etc/NetworkManager/NetworkManager.conf
    为
    [ifupdown]
    managed=true

    启动服务
    service network-manager start
````
3. 然后在GUI上Edit Connections,  
尝试Wi-Fi Security下面各种组合.  
最后得到的是  
````
    Security: WPA & WPA2 Enterprise
    Authentication: Protected EAP(PEAP)
    Anonymous identity: 空
    Domain: 空
    CA certificate: (无法勾选)
    CA certificate password: (无法勾选）
    选上No CA certificate is required 　则上面两个选项就会无法勾选

    PEAP version: Automatic
    Inner authentication: MD5.
    Username: [提供的USERNAME]
    Password: [提供的PASSWORD]
````

