---
title: 内网穿透
sidebar_position: 5
---

# 内网穿透

**适用于？**

适用于没有公网 IP 的家里云。

## 常见内网穿透工具

内网穿透的工具有很多，比如：

- [樱花内网穿透](https://www.natfrp.com/)
- [OpenFrp](https://www.openfrp.net/)

由于内网穿透工具较多，用户常被称为“rz 用户”，因此使用某些工具可以作为判定一个人技术水平的参考。

如果你想要在 Linux 上自建 frp，可以参考[此页面](/advance/Linux/frp)。

## 可能的问题

内网穿透后，玩家的连接会被传递到你机器上的内网穿透软件上，这个软件通过类似代理的方式访问本地的服务器。因此，服务端可能会误认为是本地连接，从而引发以下问题：

### 登录插件

#### 玩家注册

使用 IP 限制注册账号的插件（如 Authme、CMI）可能会出现问题。因为内网穿透后，所有玩家的 IP 地址会显示为回环地址（通常是 127.0.0.1），导致他们无法成功注册账号。

#### 自动登录

依赖 IP 识别的自动登录功能也会受到影响，普通玩家和 OP 玩家都会被识别为同一 IP，存在“绕过登录漏洞”的风险，可能导致用户进行不当操作。

### 显示玩家 IP 归属地

某些能够显示玩家 IP 归属地的插件在内网穿透后将失效，因为所有玩家的 IP 地址均为回环地址，不能准确显示。

### ban-ip ban 掉所有人

因为内网穿透使所有玩家的地址相同，如果使用 ban-ip 功能，将导致所有玩家（包括你自己）都被禁用。

### 反假人插件

无论是通过 IP 封禁还是白名单，反假人插件在内网穿透后几乎无法正常使用，因为所有玩家的 IP 地址相同，导致插件无法识别有效的 IP。

### 解决 IP 地址显示问题的方法：Proxy Protocol

frp 在转发玩家请求时重写请求头，会导致上述问题。虽然 frp 无法避免重写这个请求头，但可以通过 Proxy Protocol 恢复请求头，使服务器正常显示连接的 IP 地址。Proxy Protocol 是由 HAProxy 开发者 Willy 提出的反向代理协议，可以参考 [HAProxy 文档](http://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) 获取更多信息。需要注意的是，frp 的 Proxy Protocol 功能要求被其穿透的服务器也支持该协议，否则相关服务无法正常使用。

对于 Minecraft 服务器，支持 Proxy Protocol 的软件包括：

- BungeeCord 系列
- Paper 分支（1.18.2，仅支持 v2）
- [Geyser](../../../Java/process/mobile-player/Geyser/introduction/FAQ#frp搭建内网穿透想显示真实ip怎么办)
- Spigot 的插件 [HAProxyDetector](https://github.com/andylizi/haproxy-detector)

目前 BDS 服务器不支持此协议。
