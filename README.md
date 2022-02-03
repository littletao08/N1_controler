# n1遥控器使用说明

自己给斐讯的N1的盒子刷了YYF的电视盒子系统后，发现没有合适的遥控器，目前网上提供的遥控器有的不能完全适配，有的有很多广告弹出来，所以查看了遥控器的实现原理发现就是简单的局域网内的api调用，所以动手实现了这个app,提供给大家使用，界面是自己参照遥控器设计的，如果哪里使用不爽或者有问题欢迎提出ISSUE我会尽快修改。

 **version 1.2 更新说明** 
 - 提供了ip显示。
 - 提供apk文件下载，请在项目根目录中找到controller1.2.apk下载安装更新。（适用于下载链接404的情况，暂时提供安装包下载）
 - 使用问题欢迎继续反馈。

### 下载地址：https://fir.im/n1Controller (有时候会出现404,可能是app发布服务器在维护，请过两天再尝试下载就可以了，或者尝试备用地址)
### 备用地址：https://d.7short.com/n1Controller

### app界面截图：

![android版本](https://i.loli.net/2020/02/01/8Zzf6aTSr1MAWiv.png)

### 另外感谢值友-id:自然纯真 提供了微信小程序版本遥控器（微信小程序搜索 微遥控器）

![小程序版本](https://i.loli.net/2020/04/23/NLE7bFwhrZ4nDM8.png)

- 首次使用前请先查看你的盒子的IP地址，例如我的是192.168.168.113，在界面的右上方点击IP按钮,输入即可，当盒子连接的网络发生变化的时候需要及时更换确保遥控器生效
- 确保手机wifi和盒子处在同一个网络中，才能使遥控器生效
- 卸载或者清除缓存后请重新设置ip地址
- 该app无法实现n1开机，但是可以关机，因为我从不关机我也不是很关心这个，可以买个智能插座进行开机控制。


### 接口说明

URL:http://N1的IP:8080/v1/keyevent  (发送按钮指令)
方式：POST
参数内容：

{"keycode":按键代码,"longclick":false}
按键代码列表：

- 上:19
- 下:20
- 左:21
- 右:22
- 返回:4
- 音量加:24
- 音量减:25
- 主界面:3
- 菜单:82
- 确认键:23
- 电源:26

URL: http://N1的IP:8080/v1/action (打开设置界面)
方式：POS
参数内容：

{"action":"setting"}


祝大家使用的开心!
后来想到官改系统已经具有了root权限，而且开放了telnet服务(端口号为2323)，完全可以使用Linux命令实现关机。

基本思路就是用java编写telnet客户端，远程登录到电视盒子上，执行

echo mem > /sys/power/state

命令（具体介绍请百度“安卓电源管理”）关闭屏幕，注意这里只是休眠，wifi仍连接着，并不是关机，要是真的关机就没办法唤醒了！虽然这个办法也不是那么完美，但是毕竟省去了拔电源的苦恼，而且n1待机状态下耗电量很小，可以忽略。

想要亮屏的话执行下面的命令即可

echo "mem disk" > /sys/power/state
