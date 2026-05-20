# VPS 国家 / ASN 白名单防火墙 (vps-geo-asn-firewall)

`ip.sh` —— Debian 13 VPS 防火墙安全精简版，按**国家**与 **ASN** 做入站白名单，支持 IPv4 + IPv6，交互式菜单。

## 用途

只放行指定国家/地区和指定 ASN 的来源 IP 入站，其余一律 DROP，用于收紧暴露在公网的 VPS。SSH 22 端口**永远放行**，避免把自己锁在外面。

## 主要功能

- 国家白名单（IPv4 + IPv6），数据源 [ipdeny](https://www.ipdeny.com/)
- ASN 白名单（IPv4 + IPv6），通过 `whois.radb.net` 解析 route / route6
- 基于 `iptables` / `ip6tables` + `ipset`
- 默认策略：`INPUT DROP` / `FORWARD DROP` / `OUTPUT ACCEPT`，放行 lo 与 ESTABLISHED,RELATED
- 菜单：设置允许国家/ASN → 更新 IP 列表 → 应用规则 → 清空规则

## 使用方法

```bash
sudo bash ip.sh
```

按菜单：
1. 设置允许国家（如 `cn us jp`）与 ASN（如 `AS4134 AS4837`）
2. 更新 IP 列表
3. 应用防火墙规则

配置保存在 `/etc/vps-fw/`（`countries.conf`、`asn.conf`）。

## 依赖与环境

- 系统：Debian 13（其他 Debian/Ubuntu 通常也可）
- 权限：**必须 root**
- 依赖：`iptables`、`ipset`、`whois`、`wget`（脚本会尝试自动安装）

## 注意事项

- 规则**非持久化**（脚本说明为「手动更新」），重启后 ipset/规则可能失效，需要重新应用或自行配置持久化
- 确认管理来源所在国家/ASN 已加入白名单，否则除 SSH(22) 外其它服务会被挡住
- IP 列表来自第三方源，更新需联网
