理解 arm 的启动过程
====
Fung.Xu 2016.10

# PC 的启动过程 

Linux 系统在pc上的启动的一般过程为：加载BIOS --> 读取MBR --> Boot Loader --> 加载内核

ARMLinux启动过程则：Bootloader --> 内核启动。


开发板上电第一步是启动固件，固件是出厂时固化好的，固件的作用是初始化一下基本的 设备，

以nand为例，固件irom初始化好sram后，将nand中的前4k的bootloader（一般为uboot） 拷贝到sram中，sram再初始化另一些设备比如dram等等，然后运行剩下的bootloader，接下来就是引导linux内核的启动了。
bios在开发板相当与irom部分功能和uboot的前4k，内存时钟会在uboot中初始化的。

启动固件一般在CPU里, 但可以编写，同一款cpu应该 bootloader 是一样的。

uboot 相当于 bios , 在设备的前4k。

每个开发板的uboot都不一样。

# 常见的 cpu 

- S3C2440 ARM9 16/32位

```
S3C2440支持两种启动模式：NAND和非NAND（这里是Nor Flash），具体采用的方式取决于OM0、OM1两个引脚的状态。

OM[1:0所决定的启动方式

OM[1：0]=00时，处理器从NAND Flash启动

OM[1：0]=01时，处理器从16位宽度的ROM启动

OM[1：0]=10时，处理器从32位宽度的ROM启动。

OM[1：0]=11时，处理器从Test Mode启动。
```

> SD = NAND Flash + 接口芯片 + PCB + 塑料壳子

- S5P4418 Cortex-A9的4核处理器 . 启动有不一样

```
Bootloader --> 2nboot --> uboot -> uImage
```

# ARM cpu 架构

|Arch  | Model         | bit | Release |
|------|---------------|-----|---------|
|ARMv8 | Cortex-A57/A53|64bit| S5P6618 |
|ARMv7 | A15/12/9/8/7/5|32Bit| A10/H3  |
|ARMv6 | ARM11         |32Bit|         |
|ARMv5 | ARM9          |32Bit|         |
|      | ARM7          |32Bit| 1994    |

ARM7系列采用冯·诺依曼体系结构
![](http://2.eewimg.cn/news/uploadfile/2016/0808/20160808092600638.jpg)

而ARM9~ARM11采用哈佛体系机构
![](http://2.eewimg.cn/news/uploadfile/2016/0808/20160808092600632.jpg)


A53 - S5P6818
A9 - S5P4418
A8 - 全志A10
A7 - H3
