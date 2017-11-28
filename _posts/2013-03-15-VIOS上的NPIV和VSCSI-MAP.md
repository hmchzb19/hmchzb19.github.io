# VIOS上的NPIV 和VSCSI MAP

VSCSI 是将storage 先MAP给VIOS，然后将VIOS看到的盘MAP给VIOS的LPAR。
而NPIV host 直接在storage 上创建，VIOS只负责虚设备，VIOS 看不到NPIV的盘。
Power 这个平台复杂度比较高，因为其实有HMC的存在，HMC 通过FSP管理Power Server, 通过RMC 和LPAR 通信。

lsmap -all

查看map 关系

--------------- -------------------------------------------- ------------------

vhost18&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;U8233.E8B.1001CFP-V1-C21&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0x0000000a

VTD&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vtscsi1<br>
Status&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Available<br>
LUN&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0x8100000000000000<br>
Backing device&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;disk_8072<br>
Physloc                <br>
Mirrored&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;N/A<br>


VTD&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vtscsi24<br>
Status&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Available<br>
LUN&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0x8400000000000000<br>
Backing device&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lp21vd3<br>
Physloc                <br>
Mirrored&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;N/A<br>


VTD&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vtscsi35<br>
Status&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Available<br>
LUN&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0x8300000000000000<br>
Backing device&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AIX71_lp21<br>
Physloc<br>
Mirrored&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;N/A<br>

出现了3个VTD，则意味着有3个MAP关系，在storage 上有3个DISK 被MAP给这个VIOS， 然后这个VIOS 把这三个盘MAP给它的LPAR Vhost18。
命令是

`mkvdev -vdev hdisk* -vadapter vhost*`

NPIV 则是VIOS 看不到磁盘的

lsmap -npiv -all

Name          Physloc                            ClntID ClntName       ClntOS
------------- ---------------------------------- ------ -------------- -------
vfchost0      U8233.E8B.1001CFP-V1-C14               14                 


Status:NOT_LOGGED_IN

创建NPIV的命令

`vfcmap –vadapter vfchost0 –fcp fcs3 `