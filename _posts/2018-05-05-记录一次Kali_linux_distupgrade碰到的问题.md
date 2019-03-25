#### 今天得到的教训，好久没有升级kali了，想升级下  

        apt-get update
        apt-get dist-upgrade
        apt-get upgrade

升级一切顺利，然后可以看到登录页面，输入root 口令后直接就卡死。 思考一下这个问题，可以看到登录界面，但是输入root口令后假死，应该是gnome 这些GUI上的问题。  

​按电源键关机，启动进入recovery mode.  

​首先是启动网卡:  `service network-manager start`  

不管那么多，直接先把gnome全部卸载掉。  
    
        apt-get purge gnome
        apt-get purge gnome*

重启进入系统，可以直接Ctrl + Alt + F[n] 选择session  
没有gnome，重新安装一个Desktop manager. 
    
        #install xfce4 on Kali linux
        apt-get install kali-defaults kali-root-login desktop-base xfce4 xfce4-places-plugin xfce4-goodies
    
        #choose x-session-manage
        update-alternatives --config x-session-manager
    

再重启，进入系统重新配置输入法:  

        rm /etc/X11/xinit/xinputrc
        im-config
        #restart gdm
        service restart lightdm



到这里，系统可以使用了，等于是GDM从Gnome 换成了xfce4.

我的结论是,kali 这个系统的GUI经常出现各种各样的问题, 以前就是问题频发。  
我感觉不要经常做dist-upgrade.  
弄一次就黑一次屏，换个GDM, 是很累的事情。   

最后看下升级完的的Ｇrub  

    
    
        root@kali:~# update-grub
        Generating grub configuration file ...
        Found background image: /usr/share/images/desktop-base/desktop-grub.png
        Found linux image: /boot/vmlinuz-4.15.0-kali3-amd64
        Found initrd image: /boot/initrd.img-4.15.0-kali3-amd64
        Found linux image: /boot/vmlinuz-4.12.0-kali2-amd64
        Found initrd image: /boot/initrd.img-4.12.0-kali2-amd64
        Found linux image: /boot/vmlinuz-4.7.0-kali1-amd64
        Found initrd image: /boot/initrd.img-4.7.0-kali1-amd64
        Found linux image: /boot/vmlinuz-4.6.0-kali1-amd64
        Found initrd image: /boot/initrd.img-4.6.0-kali1-amd64
        Found Windows 8 on /dev/sda1
        done
        root@kali:~# uname -a
        Linux kali 4.15.0-kali3-amd64 #1 SMP Debian 4.15.17-1kali1 (2018-04-25) x86_64 GNU/Linux
    


好新的kernel 