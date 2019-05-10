stackoverflow 访问太慢了，因为无法访问ajax.googleapis.com,  所以我就做了件自以为是的事情。直接写了/etc/hosts文件.  

首先是dig出ajax.loli.net的ip地址，然后将这个ip地址写入/etc/hosts.  
````
119.9.105.24 ajax.googleapis.com
````

然后重启networking  
`service networking restart`

在firefox里面用F12来查看结果，确实stackoverflow加载的速度变快了。
原因是certificate报错导致Firefox没有载入jquery的库  
````
    ajax.googleapis.com uses an invalid security certificate.
    SSL_ERROR_BAD_CERT_DOMAIN
````
结果略讽刺，客观上确实速度提升了，但我的实验并不成功
