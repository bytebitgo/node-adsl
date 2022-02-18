# 项目背景

---

> 由于国内各种厂商降本增效，对成本的进一步压缩，所以被迫一些公司从专线 到 传统IDC 再到城域网 甚至到现在的ADSL 拨号汇聚，以及一些大厂无下限的P2P。
> 
> 本项目为ADSL拨号汇聚而生（不开源，无授权，但免费，请放心使用）



### 功能特性

- [x] 支持Cento7 ,Centos8系统运行,其他系统可扩展支持

- [x] 可以绑定拨号的网卡

- [x] 支持拨号的网络类型，目前两种方式（macvlan vlan）

- [x] 支持MacVlan方式下，MAC第一次随机生成后，针对每一个拨号账户固定（防ISP封禁账户）

- [x] 支持多线路的负载均衡

- [x] 支持掉线之后再拨号,秒级完成

- [x] 支持单独管理地址网卡配置

- [x] 简单方便快捷;

### 部署架构图


<details>
<summary><strong>点开查看架构图，图一为本程序，图二为ikuai</strong></summary>

![本程序架构图](https://raw.githubusercontent.com/bytebitgo/node-adsl/main/img/new.jpeg "new")


![Ikuai架构图](https://raw.githubusercontent.com/bytebitgo/node-adsl/main/img/old.png "old")

</details>


# 最近更新

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

