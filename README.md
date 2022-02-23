# 项目背景

---

> 由于国内各种厂商降本增效，对成本的进一步压缩，所以被迫一些公司从专线 到 传统IDC 再到城域网 甚至到现在的ADSL 拨号汇聚，以及一些大厂无下限的P2P。
> 
> 本项目为ADSL拨号汇聚而生（不开源，无授权，但免费，请放心使用）

>后续会持续更新新功能和修复bug ，会定期同步下载地址。

>催更联系 bytebit@foxmail.com

### 功能特性

- [x] 支持Cento7 ,Centos8系统运行,其他系统可扩展支持

- [x] 可以绑定拨号的网卡

- [x] 支持拨号的网络类型

     - 1. 所有拨号账号都在一个 vlan, 配置文件例子 ![user.txt](https://raw.githubusercontent.com/bytebitgo/node-adsl/main/user.txt_novlanid "new")


     - 2. 每个拨号账号对应一个 vlan, 配置文件例子 ![user.txt](https://raw.githubusercontent.com/bytebitgo/node-adsl/main/user.txt_vlanid "new1")


     - 3. 同一个vlan下，对应不同或相同拨号账户的多拨（运营商不限制情况下） 配置文件例子 ![user.txt](https://raw.githubusercontent.com/bytebitgo/node-adsl/main/user.txt_vlanid_equal "new2")


- [x] 支持MacVlan方式下，MAC第一次随机生成后，针对每一个拨号账户固定（防ISP封禁账户）

- [x] 支持多线路的负载均衡

- [x] 支持掉线之后再拨号,秒级完成

- [x] 支持单独管理地址网卡配置

- [x] 简单方便快捷;

### 部署架构图


<details>
<summary><strong>点开查看架构图，图一为本程序，图二为ikuai</strong></summary>

![本程序架构图](https://raw.githubusercontent.com/bytebitgo/node-adsl/main/img/new.jpeg "new")

</details>


# 最近更新

- 🚀 支持重复用户(一个账户 多拨，运营商不限制情况下)  Wed, 23 Feb 2022 09:34:42 +0800

- 🚀 增加新模式 macvlanMix Tue, 22 Feb 2022 21:22:04 +0800

- 🚀 vlan操作逻辑更改 Mon, 29 Nov 2021 10:05:48 +0800
  

- 🐛网关ip添加策略更改 Wed, 24 Nov 2021 17:22:12 +0800
  

- 🌟mac 随机后三个byte Mon, 22 Nov 2021 10:34:53 +0800
  

- 🐛添加vlan前,先删除Wed, 10 Nov 2021 14:50:36 +0800🚀固定macaddr bug fix Wed, 10 Nov 2021 14:27:29 +0800

- 🌟固定macadrr, 回写到user.config Tue, 19 Oct 2021 10:49:40 +0800
  

- 🌟支持vlan 模式  Mon, 18 Oct 2021 17:31:57 +0800
  

- 🚀ping 之前实时获取对应的ip&gate Tue, 31 Aug 2021 15:14:21 +0800
  

- 🐛log error bug  Tue, 31 Aug 2021 14:54:59 +0800
  

- 🚀主动ping网关  Tue, 31 Aug 2021 14:21:37 +0800
  

- 🐛pid 查找bug  Thu, 26 Aug 2021 14:40:45 +0800
  

- 🚀1.多个默认网关问题 2.改名 Fri, 28 May 2021 10:17:54 +0800

### 安装与配置

# Centos7系统

```shell
yum -y install rp-pppoe ppp
```

# Centos7初始化脚本

```shell
curl -s curl -s https://mirrors.xiangyundns.cn/init/ppp/el7-ppp.sh 2>/dev/null|bash -
```

# Centos8系统

```shell
yum -y install NetworkManager-ppp ppp
```

# Centos8初始化脚本

```shell
curl -s https://mirrors.xiangyundns.cn/init/ppp/el8-ppp.sh 2>/dev/null|bash -
```

# 配置代理,方便远程目标设备

```shell
curl -s https://mirrors.xiangyundns.cn/init/frp/inst-frp.sh|bash
```

#### 修改 /etc/frp/frpc.ini

- 配置修改 frpc.ini 内容(***关于frp的使用，不在这里讲，自行baidu***)
  
  

remote_port 字段(必须使用未使用的端口)

## 配置文件如下：

```ini
[ctc-ly-1011-SSH]

type = tcp

local_ip = 127.0.0.1

local_port = 22

remote_port = 7195

use_encryption = true

use_compression = true

[ctc-ly-1011-SNMP]

type = udp

local_ip = 127.0.0.1

local_port = 161

remote_port = 7196

use_encryption = false

use_compression = false
```

## 重启 frpc

```shell
systemctl restart frpc
```

# 安装 pppmgt

```shell
rpm -ivh https://mirrors.xiangyundns.cn/init/ppp/pppmgt-1.0.0-15.el8-x86_64.rpm
```



### ~~主配置文件: /etc/pppmgt/config.yaml~~

```yaml
eths: [eno1] # 拨号绑定的网卡

net_type: vlan # 选择 macvlan 或 vlan

mgtnet: # 管理网卡配置，如果配置了，则 加入策略路由，从默认网关中移除

ip: '192.168.100.219'

gate_ip: '192.168.100.1'

eth: 'eno3'

bind_ip: ['112.27.240.52','218.15.40.171']
```

# 账号密码文件: /etc/pppmgt/user.txt

#### `user password vlanid`

#### (vlan模式下必须,macvlan 模式下使用中横线代替"-") MacAddress(程序会自动填写,无须手动填写)**

#### <mark>user password vlanid</mark>(vlan模式下必须) `mac_addr`

```textile
637132896453 888888 201

637132896231 888888 202

637132770236 888888 203
```

-  可选配置

### 将管理网卡的配置文件手动改成静态地址配置,无默认网关

```shell
/etc/sysconfig/network-scripts/ifcfg-eno3

TYPE="Ethernet"

BOOTPROTO="static"

DEFROUTE="yes"

IPV6INIT="no"

IPV6_AUTOCONF="yes"

IPV6_DEFROUTE="yes"

NAME="eno3”

DEVICE="eno3”

ONBOOT="yes"

IPADDR="192.168.100.56"

NETMASK="255.255.255.0"

DNS1="119.29.29.29"

DNS2="223.5.5.5"
```



# 启动 pppmgt 进程

```shell
systemctl start pppmgt
```

# 检查 ppp 地址数量是否符合要求

```shell
ip add list|grep "scope global ppp"|wc -l
```

# pppmgt故障相关问题排查

```shell
cat /var/log/pppmgt/ppp.log
```



# ppp 拨号问题排查

```shell
cat /var/log/ppp/pppoe-ppp0.log

......

cat /var/log/ppp/pppoe-pppN.log
```