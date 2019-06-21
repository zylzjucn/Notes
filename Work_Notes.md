### 06.14

> ***熟悉docker以及tigergraph***

加入镜像
> docker load ...

加入ps -a
> docker run ...

加入ps
> docker start

> docker attach \<imagename>

### 06.20

> ***根据仓库，配置远程服务器的环境，下载图像描述中文数据集***

查看pip安装的库

> pip3 list
> pip3 list | grep tensorflow

进入虚拟环境

> source bin/activate

退出虚拟环境

> deactivate

虚拟机环境下一般不需要用到sudo。如果用到sudo，就证明已经在大环境下操作了

启动visdom

> python -m visdom.server

ssh 传输文件

> scp /path/to/file username@a:/path/to/destination
> 
> scp username@b:/path/to/file /path/to/destination
