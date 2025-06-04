# pacproxy-server

pacproxy runs in a web server 在vps服务器上运行的pacproxy

自动获取SSL数字证书, 自动加载SSL数字证书

关于pacproxy参见[pacproxy.js](https://github.com/httpgate/pacproxy.js)


## 准备

* 需要能运行nodejs的服务器, 建议选用Debian服务器

* 需要[申请一个域名](https://github.com/httpgate/pacproxy.js/blob/main/documents/About_Domain_ZH.md)，并将域名指向服务器公网IP

* 需要能够临时占用服务器公网IP的80端口，或者Cloudflare管理的域名需要获取[DNS API Token](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/), 其他域名可以[托管到Cloudflare](https://developers.cloudflare.com/fundamentals/setup/manage-domains/add-site/)

* 需要ssh到服务器的命令行，推荐用[Bitvise SSH Client](https://bitvise.com/ssh-client-download)

* 需要准备服务器设置文件current.site.cfg, 空白文件夹下第一次运行服务器会生成该文件样板，详情参考[样板](current.site.cfg)


## 推荐用PM2运行

推荐用pm2[直接运行npm库](https://github.com/httpgate/resouces/tree/main/pm2_Run_Npm_Package.md)

不需要等待编译好的软件，可直接运行最新的版本。


## 运行(以Windows为例)

用文本编辑器修改 current.site.cfg 后， 双击server-win.exe运行

家庭路由器需要设置端口映射，把外网80, 443端口映射到服务器的8080，8443或自己设定的端口

核对参数正确后，在浏览器访问申请的域名，第一次访问会自动获取数字证书

关闭软件再重新运行，如果运行正常会显示 pacurl, 在浏览器访问 pacurl ， 如正常访问则可以使用


## 运行(以linux为例)

### 修改网站参数设置current.site.cfg：

```
mkdir website
nano current.site.cfg 
```


### 设置share参数

* 如果启用[pacproxy-share模块](https://github.com/httpgate/pacproxy-share)需要设置share参数， 此时website参数只能设置为true, false 或 ''

* 如果不启用CDN加速，则只需要设置根路径,如 share : ['/share_path']

* 如果启用了CDN加速，则需要将CDN域名也加上,如 share : ['/share_path', 'CDN_DOMAIN']


### 设置website参数

* current.site.cfg里的website参数可以为空'', 可以是外部网站URL, 可以是true或false

* 设置为true或false时需要先在当前文件夹下创建website文件夹：mkdir website

* 设置为true时website文件夹可放一个静态网站，或一些可下载的文件。可以用chrome浏览器访问某个网页，然后保存到website文件夹里，类型选”Webpage, Complete", 文件名为：index.html

* 设置为false时会将website显示为一个文件夹，列出website里的所有文件。此时建议设置website_auth参数，保护信息隐私

* 设置website为外部网站URL时会将该网站的内容显示到自己的网站上，此时需要留意黑客DDOS攻击，会导致自己IP因为中转DDOS攻击被该外部网站和一些CDN屏蔽。此时可设置website_auth参数, 或改为使用本地网站。

* website参数设置好后，浏览器访问： https://your.site.domain 可以显示网站的内容，也可以将一些需要分享给他人下载的文件放到website文件夹内


### 运行pacproxy服务：

```
sudo ./server-linux
```
核对屏幕上显示出的PAC链接，如果不对则需要再修改current.site.cfg

从浏览器访问网站，确认运行正常后 Ctrl + C 退出


### 后台运行pacproxy服务：

```
nohup sudo ./server-linux &
```
加nohup防止关闭ssh连接后服务中止, (如nohup有问题可以改用screen)

查看日志：

```
tail -f nohup.out
```

### 停止pacproxy服务

```
ps -ef | grep server-linux
sudo kill -9 找到的pid
```

### 更新数字证书

免费数字证书现在有效期缩短为3个月，
停止服务后，重新运行服务会自动更新数字证书后再启动代理服务。