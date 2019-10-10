小白独自尝试在Mac上起Linux虚拟机，记录过程，包括遇到的一些小白问题特此记录。

### 1. download tsinghua linux mirror
    * 打开 CentOS官网:https://www.centos.org/download/
    * 进入清华界面：https://mirrors.tuna.tsinghua.edu.cn/centos/7/isos/x86_64/
    * 选择版本：CentOS-7-x86_64-DVD-1908.iso
    * 下载：约需2h

### 2. download VMware Fusion虚拟机
    * 下载地址：https://www.vmware.com/products/fusion/fusion-evaluation.html
        * no need to use pro version, need to get a valid key online: 7HYY8-Z8WWY-F1MAN-ECKNY-LUXYX
        * 下载：约需3h

### 3. 进入安装，一切设置默认即可
    * notes：用户组wheel（cat /etc/group | grep wheel查看wheel组用户）
    * 必要时需设置：MacOS—系统偏好设置-安全性与隐私

### 4. 安装完成后，为解决ping时出现的“connect: network is unreachable“问题，需修改配置文件：
    * sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0
    * 将ONBOOT=no,改为ONBOOT=yes
    * 重启配置：service network restart

### 5. linux若需和本地进行文件夹共享，需手动安装VMware Tools，安装方法：
    * 启动对应linux虚拟机
    * 进入虚拟机->重新安装VMware Tools获取对应的cdrom文件夹
    * 命令：
        * [root@localhost ~]# mkdir /mnt/cdrom
        * [root@localhost ~]#mount /dev/cdrom /mnt/cdrom/
        * [root@localhost cdrom]# cd /tmp/ 进入tmp文件夹
        * [root@localhost tmp]# tar zxvf VMwareTools-6.5.0-118166.tar.gz //解压文件
        * [root@localhost tmp]# cd vmware-tools-distrib
        * [root@localhost tmp]# ls 看见有对应的vmware-install.pl 文件
        * [root@localhost vmware-tools-distrib]# ./vmware-install.pl //安装开始
        * 中途可能遇到现实cannot file file的情况，需要sudo yum install perl* 
        * 一路回车，安装完成。
    * 安装tools完成后，进入/mnt，看见hgfs文件夹。
    * 进入共享设置，选择本地需共享的文件夹。（尽量名称全英文，中文会有点问题）
    * 进行手动挂载：两个版本
        * mount -t vmhgfs .host:/ /mnt/hgfs/（如果报错 mount: unknown filesystem type ‘vmhgfs’，用下一个）
        * /usr/bin/vmhgfs-fuse .host:/ /mnt/hgfs -o subtype=vmhgfs-fuse,allow_other

### 6. 喜大普奔，第一阶段完成！

