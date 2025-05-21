# 用Android的Termux运行pacproxy服务

##- 安装Termux

手机安装[Termux](https://play.google.com/store/apps/details?id=com.termux) 或[下载apk文件](https://termux.dev/en/)安装

为方便操作可以给Termux安装ssh和sftp，设置密码, 并启用：

```
pkg upgrade
pkg install openssh
passwd
sshd
ifconfig
```
启用后可用[Bitvise SSH Client](https://bitvise.com/ssh-client-download) 或其他SSH工具连接，默认端口是8022, IP是ifconfig命令显示的IP

## Termux安装nodejs

```
apt update
apt upgrade
apt install nodejs
```

## 用npm安装pacproxy-https-server

```
mkdir pacproxy-https-server
cd pacproxy-https-server
npm install -g pacproxy-https-server
```

## 设置和运行pacproxy-https-server

```
pacproxy-https-server
ifconfig
nano current.site.cfg
pacproxy-https-server
```

第一次运行会生成current.site.cfg模板文件，需要修改保存此文件

建议port, proxyport设置为8443， httpport设置为8080

建议将手机ip地址改为静态地址，修改保存设置后继续测试，直到正常运行

## 后台运行pacproxy-https-server

```
npm install -g pm2
pm2 start ~/../usr/bin/pacproxy-https-server
```

## 路由器设置端口映射

登录家里路由器管理页面，设置WAN端口映射,80端口映射到手机内网ip的8080端口，8443端口映射到手机内网ip的8443端口

如果没有路由器管理权限，则可尝试将upnp设置为true，或者通过获取cloudflare token来获取数字证书。

详情[参考pacproxy服务器](https://github.com/httpgate/pacproxy-server)

## 推荐

推荐用prcproxy安全的访问以下网站：
* 明慧网：https://www.minghui.org
* 干净世界：https://www.ganjing.com
* 神韵作品: https://shenyunzuopin.com
* 大法经书: https://www.falundafa.org

