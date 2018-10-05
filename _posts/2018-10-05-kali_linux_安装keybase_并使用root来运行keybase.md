1. keybase 是什么？
可以参考这个页面给出的信息
https://zhuanlan.zhihu.com/p/23953972

2. 在kali linux上安装keybase

        curl -O https://prerelease.keybase.io/keybase_amd64.deb

        dpkg -i keybase_amd64.deb

3. 使用root来运行需要设置一些配置文件和环境变量。  
修改配置文件 /usr/lib/systemd/user/keybase.service  
新加入一行Environment=KEYBASE_ALLOW_ROOT=1  
并且我把--log-file=%h/.cache/keybase/keybase.service.log  
修改成了--log-file=/tmp/%h/.cache/keybase/keybase.service.log  


        #修改配置文件　vim /usr/lib/systemd/user/keybase.service
        Environment=KEYBASE_ALLOW_ROOT=1
        Environment=KEYBASE_SERVICE_TYPE=systemd
        ExecStart=/usr/bin/keybase --debug --log-file=/tmp/%h/.cache/keybase/keybase.service.log service

        #启动keybase
        root@kali:~/Downloads# systemctl --user daemon-reload
        root@kali:~/Downloads# systemctl --user start keybase.service
        root@kali:~/Downloads#
        root@kali:~/Downloads# systemctl --user status keybase.service
        ● keybase.service - Keybase core service
           Loaded: loaded (/usr/lib/systemd/user/keybase.service; disabled; vendor prese
           Active: active (running) since Fri 2018-10-05 09:40:44 CST; 11s ago
         Main PID: 5010 (keybase)
            Tasks: 14 (limit: 4915)
           Memory: 9.8M
           CGroup: /user.slice/user-0.slice/user@0.service/keybase.service
                   └─5010 /usr/bin/keybase --debug --log-file=/tmp//root/.cache/keybase/

        Oct 05 09:40:44 kali systemd[1182]: Starting Keybase core service...
        Oct 05 09:40:44 kali keybase[5010]: 2018-10-05T09:40:44.641944+08:00 ? [DEBU key
        Oct 05 09:40:44 kali systemd[1182]: Started Keybase core service.

        #export 环境变量
        export KEYBASE_ALLOW_ROOT=1
        run_keybase
        #如果发现报错(此时可以正常launch keybase)
        /usr/bin/run_keybase: line 177: /root/.cache/keybase/keybase.redirector.log: No such file or directory

        ＃可以写一个config.json
        root@kali:~/Downloads# cat /etc/keybase/config.json
        {
        "disableConfigKey":true
        }

        #再次尝试启动run_keybase就不会看到报错了

