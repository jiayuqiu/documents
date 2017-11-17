本文记录下Ubuntu Server 16.04修改IP、DNS、hosts的方法

\--------

**1. Ubuntu Server 16.04修改IP**

```
sudo vi /etc/network/interfaces
```

回显：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet static
        address 192.168.0.88
        netmask 255.255.255.0
        network 192.168.0.0
        broadcast 192.168.0.255
        gateway 192.168.0.1
        # dns-* options are implemented by the resolvconf package, if instatlled
        dns-nameservers 192.168.0.1
        dns-search pcat
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

把address后的ip修改为自己想设定的ip后，保存退出。

之后，重启网络：

```
sudo /etc/init.d/networking restart
```

ps，ubuntu还有别的重启方式（但不一定有效）

```
sudo service network restart
```

desktop版可以这样重启：

```
sudo service network-manager restart
```

 如果只是修改了某个网卡(例如eth0)的信息，也可以通过重启网卡的方式使其修改生效。

```
sudo ifdown eth0
sudo ifup eth0
```

 最后，我在自己机子上试了重启网络或者网卡后，ifconfig里显示的还是旧ip，而且新旧两个ip都可以ping通，只有重启机子后才会显示新的ip。

 

**2.  Ubuntu Server 16.04修改DNS**

```
sudo vi /etc/resolvconf/resolv.conf.d/base
```

修改内容：

```
search localdomain #如果本Server为DNS服务器，可以加上这一句，如果不是，可以不加
nameserver 8.8.8.8 #希望修改成的DNS
nameserver 114.114.114.114 #希望修改成的DNS
```

保存退出，重启网络：

```
sudo /etc/init.d/networking restart
```

查看当前DNS：

```
cat /etc/resolv.conf
```

 

**3.  Ubuntu Server 16.04修改hosts**

查看hostname：

```
cat /etc/hostname
```

记住其hostname，修改hosts：

```
sudo vi /etc/hosts
```

前两行修改为：

```
#第1行默认这个
127.0.0.1       localhost
#第2行为你修改的ip    你刚才查看的hostname
192.168.11.52       pcat
```

保存退出。
