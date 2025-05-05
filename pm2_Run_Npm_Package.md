# 用PM2运行NPM库

PM2能在服务异常退出时自动重启服务，保证服务稳定运行。

NPM库一般经过测试，与直接下载Github代码运行相比，更加安全可靠。且可随时更新到最新版本。

以[pacproxy-https-server](https://www.npmjs.com/package/pacproxy-https-server)为例：

* 先在操作系统上安装好[nodejs和npm](https://nodejs.org/en/download)

* 安装 PM2： sudo npm install -g pm2

* 安装 pacproxy-https-server：  sudo npm install -g pacproxy-https-server

* 创建一个运行文件夹，编辑保存current.site.cfg, 运行测试 pacproxy-https-server, 直到运行一切正常。空白文件夹在第一次运行时会生成current.site.cfg文件。
```
mkdir pacproxy-https-server
cd pacproxy-https-server
sudo pacproxy-https-server
nano current.site.cfg
```

* 测试正常后，用PM2运行服务, 并查看运行结果
```
sudo pm2 start pacproxy-https-server
sudo pm2 logs pacproxy-https-server
```

* 可用pm2每天下午13点45分(举例)重启服务，清理内存, 更新数字证书：
```
sudo pm2 restart pacproxy-https-server --cron-restart="45 13 * * *"
```

* 更新NPM库到最新版本， 并查看当前版本：
```
sudo npm update -g pacproxy-https-server
sudo npm list -g pacproxy-https-server
```

* 其他NPM库运行也类似，如果要带参数运行，可在 -- 后增加运行参数：
```
sudo npm start wssagent -- [WSSURL] [PORT] -S
sudo npm start pacproxy-js -- ./example.site.domain/product.cfg
```

* 如果需要机器重启时恢复PM2的当前任务
```
sudo npm save
sudo npm startup
```