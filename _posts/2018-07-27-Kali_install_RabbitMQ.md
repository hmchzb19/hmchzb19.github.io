#### 记录安装RabbitMQ 碰到的Dependency 的问题


1. 执行`apt-get install rabbitmq-server` 报错。  

    
    
        apt-get install rabbitmq-server
    
        erlang17-xmerl : conflicts: erlang-xmerl but 1:20.3.8.1+dfsg-1 is to be installed
    


2. 需要把这些erlang17 开头的conflict 先卸载，再继续进行安装。  

        dpkg -l |grep erlang
        apt-get purge erlang17-base erlang17-crypto erlang17-asn1
        apt-get purge 


3. 完成安装rabbitmq-server  

        
        apt-get update
        apt-get install rabbitmq-server

        #verify
        root@kali:~# dpkg -l |grep rabbitmq
        ii rabbitmq-server
        
        root@kali:~# dpkg -l |grep erlang
        ii  erlang-asn1                                       1:20.3.8.1+dfsg-1                amd64        Erlang/OTP modules for ASN.1 support
        ii  erlang-base                                       1:20.3.8.1+dfsg-1                amd64        Erlang/OTP virtual machine and base applications
        ii  erlang-corba                                      1:20.3.8.1+dfsg-1                amd64        Erlang/OTP applications for CORBA support
        ii  erlang-crypto                                     1:20.3.8.1+dfsg-1                amd64        Erlang/OTP cryptographic modules
        ii  erlang-diameter                                   1:20.3.8.1+dfsg-1                amd64        Erlang/OTP implementation of RFC 6733 protocol
        ii  erlang-edoc                                       1:20.3.8.1+dfsg-1                amd64        Erlang/OTP module for generating documentation
        ii  erlang-eldap                                      1:20.3.8.1+dfsg-1                amd64        Erlang/OTP LDAP library
        ii  erlang-erl-docgen                                 1:20.3.8.1+dfsg-1                amd64        Erlang/OTP documentation stylesheets
        ii  erlang-eunit                                      1:20.3.8.1+dfsg-1                amd64        Erlang/OTP module for unit testing
        ii  erlang-ic                                         1:20.3.8.1+dfsg-1                amd64        Erlang/OTP IDL compiler
        ii  erlang-inets                                      1:20.3.8.1+dfsg-1                amd64        Erlang/OTP Internet clients and servers
        ii  erlang-mnesia                                     1:20.3.8.1+dfsg-1                amd64        Erlang/OTP distributed relational/object hybrid database
        ii  erlang-nox                                        1:20.3.8.1+dfsg-1                all          Erlang/OTP applications that don't require X Window System
        ii  erlang-odbc                                       1:20.3.8.1+dfsg-1                amd64        Erlang/OTP interface to SQL databases
        ii  erlang-os-mon                                     1:20.3.8.1+dfsg-1                amd64        Erlang/OTP operating system monitor
        ii  erlang-parsetools                                 1:20.3.8.1+dfsg-1                amd64        Erlang/OTP parsing tools
        ii  erlang-public-key                                 1:20.3.8.1+dfsg-1                amd64        Erlang/OTP public key infrastructure
        ii  erlang-runtime-tools                              1:20.3.8.1+dfsg-1                amd64        Erlang/OTP runtime tracing/debugging tools
        ii  erlang-snmp                                       1:20.3.8.1+dfsg-1                amd64        Erlang/OTP SNMP applications
        ii  erlang-ssh                                        1:20.3.8.1+dfsg-1                amd64        Erlang/OTP implementation of SSH protocol
        ii  erlang-ssl                                        1:20.3.8.1+dfsg-1                amd64        Erlang/OTP implementation of SSL
        ii  erlang-syntax-tools                               1:20.3.8.1+dfsg-1                amd64        Erlang/OTP modules for handling abstract Erlang syntax trees
        ii  erlang-tools                                      1:20.3.8.1+dfsg-1                amd64        Erlang/OTP various tools
        ii  erlang-xmerl                                      1:20.3.8.1+dfsg-1                amd64        Erlang/OTP XML tools
        rc  erlang17-base                                     17.4-dfsg-1kali4                 amd64        Erlang/OTP virtual machine and base applications
        rc  erlang17-base-hipe                                17.4-dfsg-1kali4                 amd64        Erlang/OTP HiPE enabled virtual machine and base applications 