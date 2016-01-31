title: Windows10+Ubuntu双系统安装手记
date: 2016-01-31 14:27:48
tags: [Linux, 简书]
---

最近因为毕设重新回归Ubuntu，手头有一台装了Win10的`ThinkPad X240s`，最终成功完成了`Windows 10 教育版`和`Ubuntu Kylin 15.10` 的双系统配置，下文(多图慎入)是我完成整个过程的手记。

### 安装方式
Ubuntu是很多Linux初学者最理想的选择，如果你恰好对Windows系列审美疲劳或者累觉不爱，那就要听好，有三种方法助你走进Ubuntu新世界。
1. **虚拟机安装**
`原料`：Ubuntu Kylin的[ISO](http://www.ubuntukylin.com/downloads/)、[VMware](https://my.vmware.com/cn/web/vmware/downloads)或[VirtualBox](https://www.virtualbox.org/wiki/Downloads)
`优点`：一条龙服务，安全简单
`缺点`：用户体验差，隔靴搔痒

2. **Wubi安装**
`原料`：Ubuntu Kylin的[ISO](http://www.ubuntukylin.com/downloads/)、wubi.exe
`优点`：即点即用，默认Windows开机引导
`缺点`：易死机，待机和网络易出问题

3. **U盘安装**
`原料`：Ubuntu Kylin的[ISO](http://www.ubuntukylin.com/downloads/)、[UltraISO](http://cn.ultraiso.net/xiazai.html)、[EasyBCD](http://neosmart.net/EasyBCD/)、U盘(>=2G)
`优点`：简单安全，正牌双系统
`缺点`：默认Ubuntu开机引导

###安装环境
本人的原则是，既然玩就玩最正宗的，so我选择U盘镜像双系统。
![系统属性](http://7xqm74.com1.z0.glb.clouddn.com/pc_config_new.png)

###游戏开始
下面正式开始安装，step by step。
1. **数据备份**
先别着急，你备份了吗？如果你看到这里，说明你选择了风险最大的一条路，在游戏开始之前，一定要做好**数据备份**，**数据备份**，**数据备份**。

2. **创建磁盘分区**
  * 按住**Win + X**，选择**“磁盘管理”**:
![磁盘管理概览](http://7xqm74.com1.z0.glb.clouddn.com/disk_overall.PNG)
  * 选择剩余空间较大的可分配磁盘，右键并选择**“压缩卷”**，这里选择压缩E盘50G左右的空间：
![压缩卷](http://7xqm74.com1.z0.glb.clouddn.com/disk_compress.PNG)
  * 点击“压缩”之后，E盘后部出现黑色的50G**“未分配空间”**：
![50G未分配空间](http://7xqm74.com1.z0.glb.clouddn.com/disk_compressed.PNG)
至此，磁盘分区过程完成。

3. **禁用快速启动(可选)**
  * 按住**Win + X**(请记住这个万能的组合)，选择**“电源选项”**，依次执行：
!["选择电源按钮的功能"](http://7xqm74.com1.z0.glb.clouddn.com/power_1.PNG)
!["更改当前不可用的设置"](http://7xqm74.com1.z0.glb.clouddn.com/power_22.PNG)
![取消选择"启用快速启动(推荐)"](http://7xqm74.com1.z0.glb.clouddn.com/power_33.PNG)

 注： “快速启动”是Windows 8时代引进的新特性，建议关闭该特性的原因是，“快速启动”会影响Grub开机引导过程，可能出现无法载入Ubuntu的状况，最后选择“保存修改”。

4. **禁用安全启动(Secure Boot)**
  * 进入系统“设置”，依次执行：
![更新和安全](http://7xqm74.com1.z0.glb.clouddn.com/setting_1.PNG)
![恢复>高级启动>立即重启](http://7xqm74.com1.z0.glb.clouddn.com/setting_2.PNG)
![疑难解答](http://7xqm74.com1.z0.glb.clouddn.com/setting_3.jpg)
![高级选项](http://7xqm74.com1.z0.glb.clouddn.com/setting_4.jpg)
![启动设置](http://7xqm74.com1.z0.glb.clouddn.com/setting_5.jpg)
执行重启(并不是什么都不做)。
![启动设置](http://7xqm74.com1.z0.glb.clouddn.com/setting_7.jpg)
大部分机器默认是关闭Secure Boot的，如果不放心，直接重启进Boot，简单粗暴：
![Boot>Security>Secure Boot](http://7xqm74.com1.z0.glb.clouddn.com/setting_8.jpg)
![确保Secure Boot是Disabled](http://7xqm74.com1.z0.glb.clouddn.com/setting_9.jpg)

  注：同样的，“安全启动”也是Windows 8时代为了防范RootKit病毒所采取的安全措施，但也阻止了Windows和其他操作系统的双启动，因此在载入Ubuntu镜像之前，务必确保“安全启动”已禁用。

5. **制作Ubuntu的启动U盘**
  （用U盘安装过操作系统的童鞋可以跳过这一步）
  * 备份待写入的U盘
  * 进入UltraISO，打开文件：
![打开镜像文件](http://7xqm74.com1.z0.glb.clouddn.com/ultraiso_1.png)
  * 启动>写入硬盘映像：
![写入硬盘映像](http://7xqm74.com1.z0.glb.clouddn.com/ultraiso_2.png)
  * 按默认值写入：
![写入硬盘映像](http://7xqm74.com1.z0.glb.clouddn.com/ultraiso_3.png)
  * 完成写入：
![完成硬盘映像写入](http://7xqm74.com1.z0.glb.clouddn.com/ultraiso_4.png)

6. **U盘安装Ubuntu**
  万事俱备，跟往常重装系统一样，插上U盘，根据机器找到进入Boot的快捷键(我的是F1)，下面step by step详解。
  * 找到镜像U盘，调整Priority Order，Save and Exit：
![Boot Priority Order](http://7xqm74.com1.z0.glb.clouddn.com/boot_2.jpg)

  注：下次restart记得重置，否则无限循环。
![炫酷的起始画面](http://7xqm74.com1.z0.glb.clouddn.com/boot_3.jpg)
  * 选择“安装Ubuntu Kylin”：
![欢迎页](http://7xqm74.com1.z0.glb.clouddn.com/boot_4.jpg)
  * 完成默认设置：
![准备事项](http://7xqm74.com1.z0.glb.clouddn.com/boot_5.jpg)

  注：如果网络和空间匀速，建议选择“安装中下载更新”和“安装这个第三方软件”。

  * 非常重要的一步，选择**“其他选项”**：
![安装类型选择](http://7xqm74.com1.z0.glb.clouddn.com/boot_6.jpg)
  * 为空闲磁盘分区：
  在这一步会看到我们之前分配的未使用磁盘空间，我们即将为这块空闲磁盘分区，为了更方便理解接下来的操作，这里简单介绍一下安装过程所涉及到的几个主要的Linux分区：
  `/`：存储系统文件，建议10GB ~ 15GB；
  `swap`：交换分区，即Linux系统的虚拟内存，建议是物理内存的2倍；
   `/home`：home目录，存放音乐、图片及下载等文件的空间，建议最后分配所有剩下的空间；
  `/boot`：包含系统内核和系统启动所需的文件，实现双系统的关键所在，建议200M。

 ![选择空闲磁盘](http://7xqm74.com1.z0.glb.clouddn.com/boot_7.jpg)
  选定空闲磁盘，点击**+**，首先分配16G空间给/分区，选择**“主分区”**、**“空间起始位置”**、**Ext4**和**“挂载点/”**：
![创建/分区](http://7xqm74.com1.z0.glb.clouddn.com/boot_8.jpg)

  注：实际上，一块硬盘最多容纳4个主分区，或3个主分区外加1个扩展分区，在选择分区类型时，可能会出现“安装系统时空闲分区不可用”状况，为了解决问题，下面一律选择“逻辑分区”。
  重复创建步骤，分配16G空间给swap分区，选择**“逻辑分区”(主分区已满)**、**“空间起始位置”**、用于**“交换空间”**：
![创建交换分区](http://7xqm74.com1.z0.glb.clouddn.com/boot_9.jpg)

  接着分配200M空间给/boot分区，选择**“逻辑分区”(主分区已满)**、**“空间起始位置”**、“Ext4”和**“挂载点/boot”**：
![创建/boot分区](http://7xqm74.com1.z0.glb.clouddn.com/boot_10.jpg)

  最后将所有剩余空间分配给/home分区，选择**“逻辑分区”(主分区已满)**、**“空间起始位置”**、“Ext4”和**“挂载点/home”**：
![创建/home分区](http://7xqm74.com1.z0.glb.clouddn.com/boot_11.jpg)

  选择/boot对应的盘符作为“安装启动引导器的设备”，务必保证一致：
![确保引导设备编号和/boot一致](http://7xqm74.com1.z0.glb.clouddn.com/boot_12.jpg)
  
  将改动写入磁盘，去喝杯咖啡：
![](http://7xqm74.com1.z0.glb.clouddn.com/boot_13.jpg)

  * 咖啡喝完，刚好进入最后的设置：
![地理位置](http://7xqm74.com1.z0.glb.clouddn.com/boot_14.jpg)
![键盘布局](http://7xqm74.com1.z0.glb.clouddn.com/boot_15.jpg)
![用户信息](http://7xqm74.com1.z0.glb.clouddn.com/boot_16.jpg)
![欢迎界面](http://7xqm74.com1.z0.glb.clouddn.com/boot_17.jpg)
![大功告成](http://7xqm74.com1.z0.glb.clouddn.com/boot_18.jpg)
  
  重启系统，进入Windows完成最后的引导设置。

7. **EasyBCD引导Ubuntu**
  进入EasyBCD，选择“添加新条目”，选择Linux/BSD操作系统，在“驱动器”栏目选择接近200M的Linux分区：
![EasyBCD设置](http://7xqm74.com1.z0.glb.clouddn.com/easybcd_2.png)
  完成条目添加后，重启电脑，会发现Windows10和Ubuntu的双系统已经完成安装，祝玩得开心！
![添加之后的条目](http://7xqm74.com1.z0.glb.clouddn.com/easybcd_3.png)
![Windows引导的开机画面](http://7xqm74.com1.z0.glb.clouddn.com/launching.jpg)
  ![Ubuntu Kylin桌面](http://7xqm74.com1.z0.glb.clouddn.com/screenshot.png)

### 最后
  用Windows引导Ubuntu最大的好处就是，当不再需要Ubuntu的时候，直接在Windows磁盘管理中将其所在所有分区删除，然后将EasyBCD中对应条目删除即可。关于Ubuntu引导Windows的方法，如果大家感兴趣，欢迎尝试和分享。

### 参考
[Beginners Guide To Install Windows 10 With Ubuntu](http://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi/)
[如何在 Win8 上禁用 UEFI 安全引导以安装Linux](https://linux.cn/article-3061-1.html)
[Windows 10 Pro + Ubuntu 14.04.3 LTS 雙系統安裝](https://blog.birkhoff.me/windows-10-and-ubuntu-14_04_3-lts-dual-boot/)