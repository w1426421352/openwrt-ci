<img width="768" src="https://github.com/openwrt/openwrt/blob/main/include/logo.png"/>

# LibWrt 25.12 IPQ 云编译

本仓库保留 breeze303/openwrt-ci 的轻量风格，使用 GitHub Actions 编译
[LiBwrt/LibWrt](https://github.com/LiBwrt/LibWrt) 的 `25.12-nss` 稳定分支。

固件只增加中文 LuCI、HTTPS 管理、软件包管理器和 CA 证书。NSS、无线驱动、
默认软件包、网络配置、管理地址和密码均沿用 LibWrt 上游，不注入代理、Docker、
下载器、第三方主题或默认配置脚本。

## 支持的构建配置

| Action 选项 | 设备 | 无线 | 分区布局 |
| --- | --- | --- | --- |
| `ipq60xx-wifi` | Qihoo 360V6 | 上游 Wi-Fi + NSS | 上游默认 |
| `ipq60xx-nowifi` | ZN-M2 | 关闭 Wi-Fi，保留有线 NSS | 上游默认 |
| `ipq807x-wifi` | Redmi AX6、Xiaomi AX3600、Xiaomi AX9000 | 上游 Wi-Fi + NSS | 扩容/自定义 U-Boot 布局 |

> IPQ807x 配置不生成 stock-layout 机型。刷写前必须确认当前分区布局，不能把
> stock 与扩容布局镜像混用，也不要强制跳过 sysupgrade 兼容性检查。

## 云编译

1. 打开仓库的 **Actions** 页面。
2. 选择 **LibWrt 25.12 IPQ**。
3. 点击 **Run workflow**，选择设备配置。
4. `source_ref` 默认使用 `25.12-nss`；需要复现固定版本时可填写 LibWrt tag。
5. 编译成功后，从对应 GitHub Release 下载固件、`build.config`、
   `effective.config` 和 `Packages.tar.gz`。

手动构建默认发布 Release。分支测试构建只保存 14 天的 Actions Artifact，避免把
未经验证的测试固件发布为 Release。

## 默认设置

- 管理地址：`192.168.1.1`
- 用户：`root`
- 密码：无（首次登录后请立即设置）
- 软件包格式：APK

## 配置原则

- `configs/` 中只保存最小配置片段，其余内容由当前 LibWrt `make defconfig` 生成。
- 不混用其他 OpenWrt/ImmortalWrt 版本的 `kmod-*` 包。
- 每个 Release 的标签包含实际 LibWrt 源码提交短哈希，便于固件与软件包对应。
- 需要额外软件时，优先从同一次构建的 `Packages.tar.gz` 安装或加入配置重新编译。

## 免责声明

- 使用本固件造成的理论或实际损失由使用者自行承担。
- 禁止用于任何商业或违法用途。
