---
title: 申请公网IP
sidebar_position: 5
---

:::tip

申请公网 IP 是一件非常头疼的事情，你可能需要耗费很多精力，且与运营商周旋，甚至可能需要借助自己的人脉关系。

:::

# 申请公网 IP

<details>
  <summary>为什么要申请公网IP？</summary>

## IPv4 的枯竭和 IPv6 推广的层层阻力

IPv4 四则位表示法能够支持 4,294,967,296（约 40 亿）个地址。然而，由于某些特殊规则，如以 `192.168` 开头的 IP 地址仅限于内网使用，实际上分配给互联网上设备的 IP 地址远少于总量。面对全球超过 70 亿人口，40 亿的地址本就不足，加之各国之间的资源分配不均，美国国防部和贝尔实验室就拥有大量 IPv4 地址，而中国大陆只有约 4 亿个，无法满足 14 亿人的需求。

为了解决 IPv4 地址短缺的问题，引入了 NAT 技术。NAT 的原理是将多台设备置于一个局域网中，局域网内的设备不直接获得公网 IP，只通过 NAT 网关来连接互联网。

然而，局域网中的设备如果需要作为服务器，仍然可以通过两种方式实现：

1. **端口映射**：NAT 网关主动将其公网 IP 的某些端口流量转发到局域网内特定设备的端口。
2. **NAT 打洞**：通过客户端和服务器间的双向连接，让 NAT 的网络设备直接建立连接。

然而，由于越来越多的家庭和公司网络使用 NAT，IPv4 地址变得愈加紧缺，同时各大运营商因使用便捷，也更倾向于维护现有的 NAT 架构，阻碍了 IPv6 的推广。

IPv6 地址格式长，从理论上说，其地址数量庞大到足以满足未来的需求，但因与 IPv4 不兼容，推广过程困难重重。许多老旧设备无法兼容新的协议，导致推广进程缓慢。

随着用户数量的不断增加，运营商为了降低成本而选择不主动推动 IPv6，使得许多家庭宽带设备依然处于 NAT 网络下，难以自行搭建服务器。

</details>

<details>
  <summary>开通 IPv6 的方法</summary>

## 开通 IPv6

国内手机网络在大多数地区都支持 IPv6，但部分家庭宽带（如网吧、酒店等公共网络）仍然不支持。如果想查找自己网络是否支持 IPv6，可以访问 ipw.cn 或 testipv6.cn。如果不支持，可能需要手动进行操作来开启。以下是一些可能的方法（2024 年）：

1. **检查是否本身已经有 IPv6**：
   - 如果是 Windows 系统，打开任务管理器，点击性能选项卡，找到以太网查看是否有 IPv6 地址，且前缀不是 `fe80`。
2. **检查宽带是否具备 IPv6 支持**：

   - 登录路由器后台，查找 IPv6 设置。比如在华为路由器中可在网络设置中找到相关选项，确保 IPv6 功能开启。

3. **确认路由器接入方法**：

   - 确保路由器的连接方式为拨号上网（PPPoE），如果是 DDNS 方式，需要修改光猫设置为桥接模式。

4. **修改光猫设置**：

   - 如果没有修改权限，可以尝试联系运营商更改，或者自己寻找光猫的超级密码进行操作（确认前先调查相关设置方法）。

5. **使用路由器进行拨号**：

   - 将光猫改为桥接后，在路由器中设置为拨号上网（PPPoE），填写相关账号和密码。

6. **测试**：
   - 操作完成后，返回服务器检查 IPv6 是否有效，可以通过 ipw.cn 或 testipv6.cn 验证。

至此，IPv6 的设置完成。

</details>

## 为家里的宽带申请公网 IPv4

由于 IPv4 地址枯竭，家用宽带中的服务器基本都处于 NAT 网络下，只能通过端口映射实现访问。因此，获得公网 IP 的控制权尤为关键。

### 找到主路由（NAT 网关）

首先找到您家中的弱电箱，通通常放置光猫。确保连接中有光纤与光猫并正确设置。

#### 进入路由器后台

为了确定 NAT 网关，需进入路由器后台，方法如下：

<details>
  <summary>操作系统访问路由器 IP 地址的方法</summary>

- **Windows**：控制面板->网络和共享中心->查看活动网络->点击连接的网络->详细信息->IPv4 默认网关。
- **MacOS**：设置->网络->选择连接的网络->点击详细信息->TCP/IP->路由器地址。
- **Linux**：执行 `ip route` 命令。
- **iOS**：设置->WLAN->点击已连接的网络->查找路由器 IP 地址。
- **Android**：设置->网络->找到连接的 Wi-Fi，查看网关。

</details>

进入后确认是否为路由器或光猫。

#### 登录路由器后台

通常需要输入密码，确保你了解该密码。若已无法登录：

- 联系提供宽带的服务商或上 installer。
- 重置路由器（注意！切勿重置光猫！）。

:::info

部分地区运营商可能不允许用户自行登录光猫。请直接参考[主动向运营商申请公网 IPv4](#主动向运营商申请公网-ipv4)。

:::

#### 查看 WAN IP

确认 WAN IP 是公网地址，如为 10 或 100 开头，表示需要进一步处理。

### 确认是否已有公网 IPv4

进入 NAT 网关的后台后，查看 WAN IP。如果为 10 或 100 开头，无法进行端口映射。

### 主动向运营商申请公网 IPv4

如果 WAN IP 是 10 或 100 开头，您可以尝试与运营商商谈，要求获得公网 IP。

1. 确保使用运营商为**联通或电信**。
2. 申请过程中可能面对收费条件、宽带套餐要求、报备手续等。

## 商用宽带

在无法获得公网 IP 的情况下，另一种方法是申请商用宽带。

### 条件要求：

1. 实际需要以公司名义申请，通过合法登记的企业办理。
2. 宽带费用较高，非普通家庭所能承受。

确保以上条件后，可以联系运营商申请商用宽带。

:::tip

某些地区可能会有面向小企业的低价宽带，若预算有限，可尝试联系运营商获取信息。

:::
