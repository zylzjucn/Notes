### 06.14

> ***熟悉docker以及tigergraph***

加入镜像
```
docker load ...
```

加入ps -a
```
docker run ...
```

加入ps
```
docker start
```
```
docker attach \<imagename>
```

### 06.20

> ***根据仓库，配置远程服务器的环境，下载图像描述中文数据集。上传数据集时服务器空间不够***

查看pip安装的库

```pip3 list```

```pip3 list | grep tensorflow```

进入虚拟环境

```source bin/activate```

退出虚拟环境

```deactivate```

虚拟机环境下一般不需要用到sudo。如果用到sudo，就证明已经在大环境下操作了

启动visdom

```python -m visdom.server```

ssh 传输文件

```scp /path/to/file username@a:/path/to/destination```

```scp username@b:/path/to/file /path/to/destination```


### 06.21

> ***解决了服务器空间不够的问题，但只能下载到训练集。验证集和测试集因为AIchallenger.com正在验证身份而卡住。成功使用服务器中指定GPU，从训练集图片中提取图片特征***
>
> ***成功开始炼丹，不知道味道怎么样***

指定GPU运行

```CUDA_VISIBLE_DEVICES=2 python test.py```

or

```
import os
os.environ['CUDA_VISIBLE_DEVICES']='2'
```

```
import torch as th
th.cuda.set_device(1)
```

If we have set up CUDA_VISIBLE_DEVICES. The actuall device will be numbered from zero. (4,5 -> 0,1)

每秒刷新GPU使用情况

```nvidia-smi -l 1```

