```

                       
     ______                          _____                              _
 |  ____|                        / ____|                            | |
 | |__      __ _   ___   _   _  | (___   __      __   ___     ___   | |   ___
 |  __|    / _` | / __| | | | |  \___ \  \ \ /\ / /  / _ \   / _ \  | |  / _ \
 | |____  | (_| | \__ \ | |_| |  ____) |  \ V  V /  | (_) | | (_) | | | |  __/
 |______|  \__,_| |___/  \__, | |_____/    \_/\_/    \___/   \___/  |_|  \___|
                          __/ |
                         |___/
 

```

## win10 安装 DockerToolbox

### 安装
* 下载：[DockerToolbox](https://download.docker.com/win/stable/DockerToolbox.exe)
* 双击`DockerToolbox.exe`进行安装，可以选其他盘（例如D盘），VirualBox和Git如果已经安装可以不选

```
 注：64位Windows10 pro，请考虑使用Docker Desktop for Windows      
```

### 启动
* 双击桌面图标`Docker Quickstart Terminal`初始化docker环境，等到界面出现`Downloading ...... boot2docker.iso...`关闭窗口
* 将安装目录下（如`D:\Program Files\Docker Toolbox`）`boot2docker.iso`文件拷贝至`C:\Users\Administrator\.docker\machine\cache`目录下，然后断开网络，重新启动`Docker Quickstart Terminal`即可初始化成功
* 启动成功后会显示虚拟机的相关信息，其中 `192.168.99.100`是VirtualBox中名字为`default`虚拟机的ip

### 使用putty或xshell登录虚拟机
* 将上一步得到的虚拟机ip填入`putty`或`xshell`，也可在`Docker Quickstart Terminal`或`cmd`中使用`docker-machine ls`命令得到ip
* default虚拟机的默认用户名为：`docker`，密码为:`tcuser`

### 使用镜像加速器
* 国内从Docker Hub拉取镜像有时会很慢，可以配置镜像加速器。Docker官方和国
内云服务商都提供了镜像加速器服务，例如：
1. Docker 官方中国镜像`https://registry.docker-cn.com`
2. 七牛云加速器 `https://reg-mirror.qiniu.com/`
* 在`Docker Quickstart Terminal`或`cmd`中通过`docker-machine ssh default`命令登录虚拟机（或使用putty、xshell连接），执行如下命令：
```
sudo sed -i "s|EXTRA_ARGS='|EXTRA_ARGS='--registry-mirror=加速地址 |g" /var/lib/boot2docker/profile
```
然后重启default虚拟机
```
docker-machine restart default
```

### easyswoole项目环境搭建
* 克隆sungivenplus-member项目代码到本机
```
git clone https://gitee.com/xiamen_yuan_yuan_food_stock/sungivenplus-member.git
```
* 使用composer安装依赖包，若已安装则进入sungivenplus-membert目录下直接执行`composer install --ignore-platform-reqs`
如果是win10 DockerToolbox环境还需要执行以下步骤，否则直接跳到2

> * 打开VirtualBox，选中default虚拟机，点设置  
> * 选择共享文件夹，添加共享文件夹，路径建议选择sungivenplus-member项目的上一级目录，我这里用的是data目录，sungivenplus-member中data\sungivenplus-member目录下，勾选自动挂载和固定分配，点击确定。   
> * 选择网络，网卡1下的高级，端口转发，添加一条转发规则，主机端口和子系统端口填写80其余不填，点击确定。  
> * 点击确定关闭VirtualBox，在Docker Quickstart Terminal或cmd中执行：docker-machine restart default 重启虚拟机

2. 从Registry中拉取镜像：
```
$ sudo docker pull registry.cn-hangzhou.aliyuncs.com/yehh/myeasyswoole:4.3.4

注：我这边构建的镜像环境按照线上的的环境
CentOS                        7.2.1511
swoole version                4.3.4
php version                   7.1.18
easy swoole                   3.2.4

可使用该镜像，也可自行构建
```

3  快速启动 
 ```
*  php vendor/bin/easyswoole install
*  php vendor/easyswoole/easyswoole/bin/easyswoole install
```
成功如下：
```
$ php vendor/easyswoole/easyswoole/bin/easyswoole install
  ______                          _____                              _
 |  ____|                        / ____|                            | |
 | |__      __ _   ___   _   _  | (___   __      __   ___     ___   | |   ___
 |  __|    / _` | / __| | | | |  \___ \  \ \ /\ / /  / _ \   / _ \  | |  / _ \
 | |____  | (_| | \__ \ | |_| |  ____) |  \ V  V /  | (_) | | (_) | | | |  __/
 |______|  \__,_| |___/  \__, | |_____/    \_/\_/    \___/   \___/  |_|  \___|
                          __/ |
                         |___/
EasySwooleEvent.php has already existed, do you want to replace it? [ Y / N (default) ] : n
install success,enjoy!
```

* 踩坑：[lavale china镜像去年已停用](https://packagist.laravel-china.org)，需采用新的镜像源
* 踩坑：easyswoole的版本号修改，具体参考github



4. 创建容器/挂载
```
sudo docker run -ti -p 9501:9501 \
--name myesayswoole -v \
/var/www/easyswoole:/easyswoole:rw \
registry.cn-hangzhou.aliyuncs.com/yehh/myeasyswoole:4.3.4
```

5. 启动
```
php easyswoole start
```


```
 ______                          _____                              _
 |  ____|                        / ____|                            | |
 | |__      __ _   ___   _   _  | (___   __      __   ___     ___   | |   ___
 |  __|    / _` | / __| | | | |  \___ \  \ \ /\ / /  / _ \   / _ \  | |  / _ \
 | |____  | (_| | \__ \ | |_| |  ____) |  \ V  V /  | (_) | | (_) | | | |  __/
 |______|  \__,_| |___/  \__, | |_____/    \_/\_/    \___/   \___/  |_|  \___|
                          __/ |
                         |___/
main server                   SWOOLE_WEB
listen address                 0.0.0.0
listen port                   9501
ip@eth0                       172.17.0.2
worker_num                    2
max_request                   65545
task_worker_num               1
task_max_request              500
document_root                 /easyswoole/Static
enable_static_handler         1
reload_async
task_enable_coroutine         1
max_wait_time                 3
pid_file                      /easyswoole/Temp/pid.pid
log_file                      /easyswoole/Log/swoole.log
run at user                   root
daemonize                     false
swoole version                4.3.4
php version                   7.1.33
easy swoole                   3.2.4
develop/produce               develop
temp dir                      /easyswoole/Temp
log dir                       /easyswoole/Log
```

```
sequenceDiagram
A->>B: How are you?
B->>A: Great!
```

```
graph LR
A-->B
```
