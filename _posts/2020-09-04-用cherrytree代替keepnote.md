#### kali_linux上的keepnote被drop
过去几年我用keepnote写了不少note,不知道该怎么办

第一种办法我尝试了重装keepnote,虽然repo里面没有，但是[keepnote网站](http://keepnote.org/)有下载,最后我卡在了缺少一个包`python-central`。
使用`apt --fix-broken install`这个命令卸载了keepnote,开始想怎么把keepnote里面保存的html格式的note给提取出来。

第二种方法选代替方案，可以用`cherrytree`来代替keepnote.
cherrytree的简介里面写着，`hierarchical note taking application`,
同时它支持从keepnote导入，这真是帮了我的大忙了。

安装命令：
`apt-get install cherrytree`

安装完成后打开cherrytree,选择import->From keepnote folder.
至此等于keepnote转到了cherrytree.
但是我在考虑是否应该返璞归真，用`sublime`来写txt文件当作note.
windows上则使用`notepad++`.

最后瞎扯一句，批评若不自由，则赞美无意义。


