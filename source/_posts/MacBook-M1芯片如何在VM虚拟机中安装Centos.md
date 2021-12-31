---
date: "2021-12-31"
title: MacBook M1芯片如何在VM虚拟机中安装Centos
categories: 
- 工具
tags: 
- MacBook
- VM虚拟机
- Linux
img: "https://s4.ax1x.com/2021/12/31/TfIFaV.png"
---


## 1、下载虚拟机

```tex
链接: https://pan.baidu.com/s/1au3T0QhBNb59slg-NumQnA 
提取码: tuj8 
```



## 2、下载Centos镜像

```tex
Centos7
链接: https://pan.baidu.com/s/1NqgbugIgg88yD6yAukzxWg 
提取码: 1fv5 

Centos8
链接: https://pan.baidu.com/s/1M6809M1b0_1ZRec3Blwe3A 
提取码: ogwv 
```



## 3、在VM中安装Centos

### 3.1、选择安装方法

![安装方法](https://s4.ax1x.com/2021/12/31/TfNIPS.png)





### 3.2、创建新的虚拟机

![创建新的虚拟机](https://s4.ax1x.com/2021/12/31/TfrDc8.png)

### 3.3、选择操作系统
![选择操作系统](https://s4.ax1x.com/2021/12/31/Tfc6x0.png)
![选择操作系统](https://s4.ax1x.com/2021/12/31/Tfcy2q.png)
![选择操作系统](https://s4.ax1x.com/2021/12/31/TfyOl6.png)

### 3.4、自定义虚拟机配置
![自定义虚拟机配置](https://s4.ax1x.com/2021/12/31/TfgcYd.png)
![自定义虚拟机配置](https://s4.ax1x.com/2021/12/31/TfggfA.png)
![自定义虚拟机配置](https://s4.ax1x.com/2021/12/31/Tf6Kts.png)
![自定义虚拟机配置](https://s4.ax1x.com/2021/12/31/Tf6JnU.png)
![自定义虚拟机配置](https://s4.ax1x.com/2021/12/31/Tf6YBF.png)

### 3.5、启动Centos
![启动Centos](https://s4.ax1x.com/2021/12/31/Tf6t74.png)
![启动Centos](https://s4.ax1x.com/2021/12/31/Tf6UAJ.png)

### 3.6、Centos安装信息设置
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6aN9.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6dhR.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6Dc6.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6rjK.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6ynO.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6RNd.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6h9I.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf643t.png)
![Centos安装信息设置](https://s4.ax1x.com/2021/12/31/Tf6Ijf.png)

### 3.7、开始安装
![开始安装](https://s4.ax1x.com/2021/12/31/Tf6Tu8.png)



## 4、VMware-配置静态ip

### 4.1、设置Mac的vmnet8

一般将vmnet8的IP 设置为 192.168.xxx.1 ，网关设置为 192.168.xxx.2 

- mac打开终端

  ```tex
  输入 sudo su - 
  Password：输入你的开机密码
  进入到root用户 ～root#
  ```

  

- 修改vm的网络IP配置

  ```bash
  #修改vmware 配置网卡的配置文件
  #cp复制备份
  cp /Library/Preferences/VMware\ Fusion/networking /Library/Preferences/VMware\ Fusion/networking.bck
  
  vim /Library/Preferences/VMware\ Fusion/networking      
  修改以下内容：
  
  #关闭dhcp
  answer VNET_8_DHCP no
  answer VNET_8_DHCP_CFG_HASH 705F0C419F3E2FF5E4273813FB73167BC5A93B02
  # 子网掩码
  answer VNET_8_HOSTONLY_NETMASK 255.255.255.0
  #IP段
  answer VNET_8_HOSTONLY_SUBNET 192.168.128.0
  answer VNET_8_NAT yes
  answer VNET_8_VIRTUAL_ADAPTER yes
  #设置网卡 vmnet8 的IP
  answer VNET_8_VIRTUAL_ADAPTER_ADDR 192.168.128.1
  ```

  

- 修改vm的nat网关

    ```properties
    #配置nat.conf 这个文件
    vim /Library/Preferences/VMware\ Fusion/vmnet8/nat.conf 
    
    #设定nat网管和子网
    # NAT gateway address
    ip = 192.168.128.2
    netmask = 255.255.255.0
    ```



- 重启vmnet8网卡

  ```bash
  # 查看网卡状态
  sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --status  
  
  # 关闭网卡
  sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
  
  #启动网卡
  sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start
  ```

  

### 4.2、设置centos的网卡配置

- 修改 vim /etc/sysconfig/network-scripts/ifcfg-ens160

  ```te
  记得改成你自己的网卡名称
  ```

  

- 添加如下配置

  ```properties
  TYPE=Ethernet
  #修改为静态ip，而不是dhcp的方式
  BOOTPROTO=static
  DEFROUTE=yes
  PEERDNS=yes
  PEERROUTES=yes
  IPV4_FAILURE_FATAL=no
  IPV6INIT=yes
  IPV6_AUTOCONF=yes
  IPV6_DEFROUTE=yes
  IPV6_PEERDNS=yes
  IPV6_PEERROUTES=yes
  IPV6_FAILURE_FATAL=no
  IPV6_ADDR_GEN_MODE=stable-privacy
  NAME=ens160
  UUID=bde1f735-b9b0-4a78-8513-0f364f78bdfa
  DEVICE=ens160
  #设定为网卡开机启动
  ONBOOT=yes
  #设置IP地址
  IPADDR=192.168.128.130
  #设定网关
  GATEWAY=192.168.128.2
  #设定子网掩码
  NETMASK=255.255.255.0
  #设定dsn地址
  DNS1=192.168.128.2
  ```

- 重启网络服务 

  ```bash
  service network restart
  ```

- 查看网络配置

  ```bas
  ip addr
  ```

- 查看是否通网

  ```bash
  ping baidu.com
  ```

  
