# 说明

因路由器自带uboot不能进入uboot菜单，且编译官方原版uboot也一样无法进入：

```Barsh
Please choose the operation:
   1: Load system code to SDRAM via TFTP.
   2: Load system code then write to Flash via TFTP.
   3: Boot system code via Flash (default).
   4: Entr boot command line interface.
   7: Load Boot Loader code then write to Flash via Serial.
   9: Load Boot Loader code then write to Flash via TFTP.
```
   
正常来说，uboot运行到这一步是可以通过输入对应的数字进入对应的功能的；

但由于系统版本的原因，还有串口中断的差异性，可能是usb转串口电路或者是usb转串口驱动程序。 

使得uboot运行到uboot菜单这一步，

自动接收到了一个空字节(Null)，

导致uboot直接跳过倒计时，进入3: Boot system code via Flash (default)这个选项，直接进入系统。

为了处理这个问题，我直接修改了uboot源码(BootLoader)，加入了一些功能组合按键及判断机制，

使它即使受到空字节，也不会直接进入系统，从而可以输入对应数字选择菜单对应选项.

现把修改源码后生成的patch放出来，方便有需要的朋友使用。

### 使用方式：

1.下载官方uboot源码
```Barsh
    git clone https://github.com/maxlicheng/ralink_uboot.git
```

2.更新gcc版本

3.执行补丁
```Barsh
    patch -p1 < ralink_uboot_mips_4320.patch
```
### 具体使用方式

博客《HLK-RM04 WIFI模块 uboot 编译及修改》

https://www.maxlicheng.com/github/158.html
