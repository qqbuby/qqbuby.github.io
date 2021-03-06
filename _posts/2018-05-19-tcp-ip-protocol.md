---
layout: post
title: TCP/IP Protocol
date: 2018-05-19 18:03:47 +0800
categories: ['TCP/IP']
tags: ['TCP/IP']
disqus_identifier: 80388571727017400896826908453668197145
---

- TOC
{:toc}

- - -

### 局域网上运行 HTTP 的两台主机

![局域网上运行 HTTP 的两台主机](/assets/tcp-ip-protocol/tcp-ip-http-host-on-lan.svg)

- - -

### TCP/IP 协议族中不同层次的协议 

![TCP/IP 协议族中不同层次的协议](/assets/tcp-ip-protocol/tcp-ip-family-layer.svg)

- - -

### 五类互联网地址

![五类互联网地址](/assets/tcp-ip-protocol/ip-address-class.svg)

- - -

### 数据进入协议栈时的封装过程

![数据进入协议栈时的封装过程](/assets/tcp-ip-protocol/data-segment-frame-wrapper.svg)

- - -

### IEEE 802.2/802.3 (RFC 1042) 和以太网的封装格式 (RFC 894)

![IEEE 802.2/802.3 (RFC 1042) 和以太网的封装格式 (RFC 894)](/assets/tcp-ip-protocol/ieee802.2-ethernet-frame-format.svg)

- - -

#### SLIP 报文的封装

![SLIP 报文的封装](/assets/tcp-ip-protocol/slip.svg)

- - -

#### PPP 数据帧的格式

![PPP 数据帧的格式](/assets/tcp-ip-protocol/ppp.svg)

- - -

### 环回接口处理 IP 数据报的过程

![环回接口处理 IP 数据报的过程](/assets/tcp-ip-protocol/loopback-interface-ip-packet-handling.svg)

- - -

### IP 数据报格式及首部各字段

![IP 数据报格式及首部各字段](/assets/tcp-ip-protocol/ip-data-packet-format.svg)

- - -

### 用于以太网的 ARP/RARP 请求或应答分组格式

![用于以太网的 ARP/RARP 请求或应答分组格式](/assets/tcp-ip-protocol/arp-rarp-packet-format.svg)

- - -

### ICMP 报文封装在 IP 数据报内部

![ICMP 报文](/assets/tcp-ip-protocol/icmp.svg)

- - -

#### ICMP 地址掩码请求和应答报文

![ICMP 地址掩码请求和应答报文](/assets/tcp-ip-protocol/icmp-subnet-mask.svg)

- - -

#### ICMP 不可达差错

![ICMP 不可达差错](/assets/tcp-ip-protocol/icmp-unreachable.svg)

- - -

### IP 层工作流程

![IP 层工作流程](/assets/tcp-ip-protocol/ip-working.svg)

- - -

### RIP 路由信息协议

![RIP 路由信息协议](/assets/tcp-ip-protocol/rip.svg)

- - -

### 单播, 多播, 广播

![单播, 多播, 广播](/assets/tcp-ip-protocol/uni-group-broad-cast.svg)

- - -

#### IGMP: Internet 组管理协议

![IGMP Internet 组管理协议](/assets/tcp-ip-protocol/igmp.svg)

- - -

### UDP: 用户数据报协议

![UDP 报文](/assets/tcp-ip-protocol/udp.svg)

- - -

### TCP: 传输控制协议

![TCP 报文](/assets/tcp-ip-protocol/tcp.svg)

- - -

#### TCP 的状态变迁图

![TCP 的状态变迁图](/assets/tcp-ip-protocol/tcp-status.svg)

- - -

#### TCP 正常连接建立和终止所对应的状态

![TCP 正常连接建立和终止所对应的状态](/assets/tcp-ip-protocol/tcp-open-close.svg)

- - -

#### TCP 同时打开期间报文段的交换

![TCP 同时打开期间报文段的交换](/assets/tcp-ip-protocol/tcp-simultaneous-open.svg)

- - -

#### TCP 同时关闭期间报文段的交换

![TCP 同时关闭期间报文段的交换](/assets/tcp-ip-protocol/tcp-simultaneous-close.svg)

#### RST 复位报文段

1. 到不存在的端口的连接请求

    ```sh
    $ telnet 10.200.40.55 80
    Trying 10.200.40.55...
    telnet: Unable to connect to remote host: Connection refused
    ```
    
    ```sh
    00:00:00.000000 IP 192.168.66.128.33132 > 10.200.40.55.80: Flags [S], seq 4228554322, win 29200, options [mss 1460,sackOK,TS val 1130179 ecr 0,nop,wscale 7], length 0
    00:00:01.009552 IP 192.168.66.128.33132 > 10.200.40.55.80: Flags [S], seq 4228554322, win 29200, options [mss 1460,sackOK,TS val 1130432 ecr 0,nop,wscale 7], length 0
    00:00:00.078680 IP 10.200.40.55.80 > 192.168.66.128.33132: Flags [R.], seq 970664811, ack 4228554323, win 64240, length 0
    ```
1. 异常终止一个连接

    ```sh
    $ telnet 192.168.171.1 9000
    Trying 192.168.171.1...
    Connected to 192.168.171.1.
    Escape character is '^]'.
    Connection closed by foreign host.
    ```
    
    ```sh
    00:00:00.000000 IP 192.168.66.128.37852 > 192.168.171.1.9000: Flags [S], seq 2189393428, win 29200, options [mss 1460,sackOK,TS val 1446368 ecr 0,nop,wscale 7], length 0
    00:00:00.000557 IP 192.168.171.1.9000 > 192.168.66.128.37852: Flags [S.], seq 1865883214, ack 2189393429, win 64240, options [mss 1460], length 0
    00:00:00.000067 IP 192.168.66.128.37852 > 192.168.171.1.9000: Flags [.], ack 1, win 29200, length 0
    00:00:00.019830 IP 192.168.171.1.9000 > 192.168.66.128.37852: Flags [P.], seq 1:20481, ack 1, win 64240, length 20480
    00:00:00.000152 IP 192.168.66.128.37852 > 192.168.171.1.9000: Flags [.], ack 20481, win 64240, length 0
    00:00:00.000139 IP 192.168.171.1.9000 > 192.168.66.128.37852: Flags [R.], seq 20481, ack 1, win 64240, length 0
    00:00:00.000056 IP 192.168.171.1.9000 > 192.168.66.128.37852: Flags [R], seq 1865903695, win 32767, length 0
    ```
1. 检测半打开连接

    ```sh
    $ telnet 192.168.66.131 22
    Trying 192.168.66.131...
    Connected to 192.168.66.131.
    Escape character is '^]'.
    SSH-2.0-OpenSSH_4.3
    ONE PIECE
    Connection closed by foreign host.
    ```
    
    ```
    00:00:00.000000 IP 192.168.66.128.40868 > 192.168.66.131.22: Flags [S], seq 556956684, win 29200, options [mss 1460,sackOK,TS val 1594879 ecr 0,nop,wscale 7], length 0
    00:00:00.000326 IP 192.168.66.131.22 > 192.168.66.128.40868: Flags [S.], seq 1715689874, ack 556956685, win 5792, options [mss 1460,sackOK,TS val 4294894064 ecr 1594879,nop,wscale 7], length 0
    00:00:00.000049 IP 192.168.66.128.40868 > 192.168.66.131.22: Flags [.], ack 1, win 229, options [nop,nop,TS val 1594879 ecr 4294894064], length 0
    00:00:00.011370 IP 192.168.66.131.22 > 192.168.66.128.40868: Flags [P.], seq 1:21, ack 1, win 46, options [nop,nop,TS val 4294894075 ecr 1594879], length 20
    00:00:00.000076 IP 192.168.66.128.40868 > 192.168.66.131.22: Flags [.], ack 21, win 229, options [nop,nop,TS val 1594882 ecr 4294894075], length 0
    00:03:12.727548 IP 192.168.66.128.40868 > 192.168.66.131.22: Flags [P.], seq 1:12, ack 21, win 229, options [nop,nop,TS val 1643064 ecr 4294894075], length 11
    00:00:00.002127 IP 192.168.66.131.22 > 192.168.66.128.40868: Flags [R], seq 1715689895, win 0, length 0
    ```

1. 主机不可达的连接请求(超时)

    ```sh
    $ telnet www.google.com 80
    Trying 75.126.135.131...
    telnet: Unable to connect to remote host: Connection refused
    ```
    
    ```sh
    00:00:00.000000 IP 192.168.66.128.50448 > 192.168.66.2.53: 5183+ A? www.google.com. (32)
    00:00:00.000093 IP 192.168.66.128.50448 > 192.168.66.2.53: 21632+ AAAA? www.google.com. (32)
    00:00:00.004174 IP 192.168.66.2.53 > 192.168.66.128.50448: 5183 1/0/0 A 75.126.135.131 (48)
    00:00:00.000047 IP 192.168.66.2.53 > 192.168.66.128.50448: 21632 0/0/0 (32)
    00:00:00.000239 IP 192.168.66.128.41516 > 75.126.135.131.80: Flags [S], seq 3229948700, win 29200, options [mss 1460,sackOK,TS val 1721759 ecr 0,nop,wscale 7], length 0
    00:00:01.026997 IP 192.168.66.128.41516 > 75.126.135.131.80: Flags [S], seq 3229948700, win 29200, options [mss 1460,sackOK,TS val 1722016 ecr 0,nop,wscale 7], length 0
    00:00:02.015519 IP 192.168.66.128.41516 > 75.126.135.131.80: Flags [S], seq 3229948700, win 29200, options [mss 1460,sackOK,TS val 1722520 ecr 0,nop,wscale 7], length 0
    00:00:04.256726 IP 192.168.66.128.41516 > 75.126.135.131.80: Flags [S], seq 3229948700, win 29200, options [mss 1460,sackOK,TS val 1723584 ecr 0,nop,wscale 7], length 0
    00:00:08.192206 IP 192.168.66.128.41516 > 75.126.135.131.80: Flags [S], seq 3229948700, win 29200, options [mss 1460,sackOK,TS val 1725632 ecr 0,nop,wscale 7], length 0
    00:00:05.510774 IP 75.126.135.131.80 > 192.168.66.128.41516: Flags [R.], seq 1891069686, ack 3229948701, win 64240, length 0
    ```
