1. 首先使用Director deploy 了一个 NPIV 的LPAR with Multi Disk.

2. LPAR 起来后发现HMC上的IP配置不对，修改HMC的IP配置.

3. LPAR每次重启都会报RMC没有办法链接，必须要重启LPAR上的RMC。

4. 然后再尝试去validate 这个LPAR，发现始终在报这个LPAR的SLOT 5的错，但是我查看了LPAR的profile 并没有发现有这个设备的存在，于是登入操作系统，发现。

\#lsslot -c slot

U9117.MMB.100094P-V4-C0 Virtual I/O Slot vsa0
U9117.MMB.100094P-V4-C2 Virtual I/O Slot fcs0
U9117.MMB.100094P-V4-C3 Virtual I/O Slot fcs1
U9117.MMB.100094P-V4-C4 Virtual I/O Slot ent0
U9117.MMB.100094P-V4-C5 Virtual I/O Slot vscsi0 

有vscsi0 存在，于是rmdev -d -l 删除掉，然后cfgmgr，发现这个设备重新出现了。

于是重新删除，然后关掉LPAR，重新activate 其profile,发现没有slot 5这个设备了， 重新在HMC 上VALIDATE，everything go fine now.

