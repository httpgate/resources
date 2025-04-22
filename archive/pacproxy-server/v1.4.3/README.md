# pacproxy-server

pacproxy runs in a web server 在vps服务器上运行的pacproxy

自动获取SSL数字证书, 自动加载SSL数字证书

关于pacproxy参见[pacproxy.js](https://github.com/httpgate/pacproxy.js)


## 准备

需要能运行nodejs的服务器, 新手建议选用Debian服务器

需要[申请一个域名](https://github.com/httpgate/pacproxy.js/blob/main/documents/About_Domain_ZH.md)，并将域名指向服务器IP

需要ssh到服务器的命令行，新手推荐用Bitvise SSH Client


## 运行(以Windows为例)

用文本编辑器修改 current.site.cfg 后， 双击server-win.exe运行

家庭路由器需要设置端口映射，把外网80, 443端口映射到服务器的8080，8443或自己设定的端口

核对参数正确后，在浏览器访问申请的域名，第一次访问会自动获取数字证书

关闭软件再重新运行，如果运行正常会显示 pacurl, 在浏览器访问 pacurl ， 如正常访问则可以使用


## 运行(以linux为例)

### 修改网站参数设置current.site.cfg：

```
nano current.site.cfg 
```

### 运行pacproxy服务：

```
sudo ./server-linux
```
核对屏幕上显示出的PAC链接，如果不对则需要再修改current.site.cfg

从浏览器访问网站，确认运行正常后 Ctrl + C 退出


### 后台运行pacproxy服务：

```
sudo nohup ./server-linux &
```
加nohup防止关闭ssh连接后服务中止, (如nohup有问题可以改用screen)

查看日志：

```
tail -f nohup.out
```

### 停止pacproxy服务

```
ps -ef | grep node
sudo kill -9 找到的pid
```

### 更新数字证书

免费数字证书现在有效期缩短为3个月，建议每个月更新一次数字证书

```
sudo ./server-linux forcert
```
加forcert参数运行服务后，会自动更新数字证书后再启动代理服务。