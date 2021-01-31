---
title: Linux网络配置！
date: 2021-01-21 14:36:25
tags:
- Linux
---
### 网络配置（无敌版）

<!--more-->

##### Centos网络配置

- 编辑-虚拟机网络编辑器VMNet(NAT)

  - 子网 192.168.2.0
  - 更改设置-DHCP设置：
    - 起始：192.168.2.128（1是网关，从2开始）
    - 结束：192.168.2.254（254是最大值）
  - NAT设置：网关：192.168.2.1

- 配置window访问虚拟机

  - 网络连接-vmNet8-TCP/IPV4 - IP：192.168.2.2,网关：192.168.2.1

  - centos网卡

    - 修改网卡配置：

    ```shell
    vi /etc/sysconfig/network-scripts/ifcfg-eth0
    ```

    

    ```java
    DEVICE=eth0
    HWADDR=00:0C:29:42:BA:62
    TYPE=Ethernet
    UUID=33168a7f-0441-437d-af7b-c5577a65042d
    DEFROUTE=yes
    ONBOOT=yes
    ----修改以下配置------
    NM_CONTROLLED=yes
    BOOTPROTO=static
    IPADDR=192.168.2.128
    GATEWAY=192.168.2.1
    BROADCAST=192.168.2.255
    DNS1=8.8.8.8
    DNS2=4.2.2.2
    
    ```

    - 配置网络服务

      - 停止已有的网络服务：

      ```shell
      service NetworkManager stop
      ```

      

      - 重启一个关于网卡的配置文件：

      ```shell
      /etc/init.d/network restart
      ```

      

      - 关闭开机自启，可能会破坏我们的配置信息：

      ```shell
      chkconfig NetworkManager off
      ```

      

      - 追加网关：

      ```shell
      vi /etc/resolv.conf  追加nameserver 192.168.2.1
      ```

      

      - 重启网卡：

      ```shell
      service network restart
      ```

      

