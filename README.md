# apt-upgrade-ubuntu-kernel
用标准apt方式升级Ubuntu内核并启用BBR  

看过漫谈BBR一键加速脚本repo的服务器玩家心里一定嘀咕※＃＠＆＆※＃，讲半天的都是一大堆限制条件，来点真功夫几键把内核安全升级了就不※＃＠＆＆※＃你

˃̣̣̥᷄⌓˂̣̣̥᷅     

好吧，小白拿出磨了几年的废刀把linux内核砍升天  

怎么检查内核版本的命令这里就不重复了，参照漫谈BBR一键加速脚本repo，很多开启BBR一键加速脚本都是手动下载新内核并编译安装的，但是手动升级内核存在着安全隐患，
其实 Ubuntu 官方提供了升级最新内核的方式，那就是 linux-hwe-generic 软件包。Ubuntu 通过 apt 包管理工具提供了两个内核版本，一个是通用版本 (General Availability/GA)，即最稳定的版本；一个是硬件启用版本 (Hardware Enablement/HWE)，会跟随最新的内核更新

其他linux发行版都是类似

我们可以通过输入 apt search linux-generic 看到这两个软件包。其实那些旧内核版本装的就是 linux-generic 这个最稳定版本，这个也是漫谈BBR一键加速脚本repo查内核命令举例输出的那个generic  

玩家看到这里又开始※＃＠＆＆※＃，废话一大堆，我们只要打几键，唉 〒_〒 只能马上为玩家奉上几键  

明白了上面的说明，我们就要安装 linux-generic-hwe 包即可，可以将 Ubuntu 升级为当前版本可用的最新稳定内核。Ubuntu 16.04 的包叫 linux-generic-hwe-16.04，Ubuntu 18.04 的包叫 linux-generic-hwe-18.04，如此类推。以 Ubuntu 16.04 为例，输入以下命令进行安装即可，重启后才会生效  

改用root用户  
$su  

#apt install linux-generic-hwe-16.04 -y  

#shutdown -r now  

重启后，我们可以再次输入 uname -a 查看一下内核版本，我们可以看到，此时我的 Ubuntu 16.04 已经是 4.15 版本的内核了  

------------启用 BBR------------     
虽然内核已经升级好了，但还没有正式装载 BBR 模块，还无法在可用拥塞算法中查到。运行以下命令装载 BBR

改用root用户  
$su  
#modprobe tcp_bbr  
#echo "tcp_bbr" | sudo tee -a /etc/modules-load.d/modules.conf  

然后输入命令，就能看到 bbr 了  
#sysctl net.ipv4.tcp_available_congestion_control   

输出大概是这个样子   
net.ipv4.tcp_available_congestion_control = reno cubic bbr   

看到这个输出后就可以按照漫谈BBR一键加速脚本repo的方式开启BBR了 ₍₍٩( ᐛ )۶₎₎♪  
然后就没有然后了  

这是官方推荐的方式，也是不容易出现问题的方式，还能跟随更新，何乐而不为呢  (◍•͈⌔•͈◍)   
