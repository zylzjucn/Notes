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

```
pip3 list
```

```
pip3 list | grep tensorflow
```

进入虚拟环境

```
source bin/activate
```

退出虚拟环境

```
deactivate
```

虚拟机环境下一般不需要用到sudo。如果用到sudo，就证明已经在大环境下操作了

启动visdom

```
python -m visdom.server
```

ssh 传输文件

```
scp /path/to/file username@a:/path/to/destination
```

```
scp username@b:/path/to/file /path/to/destination
```


### 06.21

> ***解决了服务器空间不够的问题，但只能下载到训练集。验证集和测试集因为AIchallenger.com正在验证身份而卡住。成功使用服务器中指定GPU，从训练集图片中提取图片特征***
>
> ***成功开始炼丹，不知道味道怎么样***

指定GPU运行

```
CUDA_VISIBLE_DEVICES=4 python main.py train
```

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

```
nvidia-smi -l 1
```

### 06.24

> ***写脚本处理excel的一天，基本完成了要做的东西***

### 06.25

> ***完善脚本。希望明天可以做回图像描述***

读

```
df = pd.read_csv(source_file, header = 0)
```

将dataframe中的数据取出来，应该是编程array

```
id1 = df['id'].values
id1name = df['name'].values
```

在某处插入一列

```
df1.insert(1, "assets_company", assets_company, True)
```

填充nan为0，并转换数据类型

```
df1['col_name'] = df1['col_name'].fillna(0).astype(np.int64)
```

遍历行，写入

```
for index, row in df1.iterrows():
    id = row['assets_company_id']
    if id in dic1:
        row['assets_company'] = dic1[id]
        df1.loc[index, 'assets_company'] = dic1[id]
```

将0换回nan

```
df1['col_name'] = df1['col_name'].replace({0:np.nan})
```

导出

```
export_csv = df1.to_csv (export_file, index = None, header=True)
```

### 06.26

> ***这一定是脚本工作的最后一天。果然是，开始学习visdom***

编译型语言，比如C/C++，执行前要先编译。以后每次执行不用编译，就直接用编译文件就好。所以快，不好跨平台。
解释型语言，比如Java, Python, Ruby，每次执行都翻译。慢，好跨平台。

### 06.27

> ***研究陈云的代码。把训练可视化跑出来了。命令行print出来了loss，visdom里面能看到正在训练以及loss的下降曲线。还可以。改了一些参数，这里的硬件实在是太快了。epoch100的weights拿到***

shell路径里面有中文还是不行

from 文件夹.文件名 import class名

### 06.28

> ***训练***

|Time|Model|Epoch|Batch_size|lr|Loss|
|--|---|----|---|---|----|
|06.28|ResNet50|160|256|1e-3~1e-5|1.033|
