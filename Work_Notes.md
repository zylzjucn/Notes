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

import fire
主函数中
fire.Fire()
即可使用命令行参数

### 06.29

> ***训练得到了resnt50和resnet102的模型。好像这个代码，不能训练CNN，只能训练RNN。下一步看看结果***

### 07.01

> ***结果不行啊。没有验证集，但是肉眼看图片输出还是不准，效果很一般***

现在确实跑通了一次，如果要继续chenyun这个模型，还有以下问题需要解决：

1. 数据标注。这是影响精度的问题之一。他的标注太长，而且我们也需要做自己的标注
2. CNN不训练。这也影响精度。这个就比较难改了，用这个模型大概率这一点改不了
3. 最后结果的输出。这个可能相对简单，看他们的要求了
4. 环境问题。torchnet等问题还是没解决，不过可以设法避免
5. 照片读取。这个不知道是不是内存问题
6. 张量报错。似乎已经解决

所以接下来，我应该会考虑用英文图像描述的已有算法，看能不能转成中文。这个事情估计也很难

### 07.02

> ***开始使用DeepRNN的新模型***

如果想引用一个文件夹里的一些包，可以直接将这些包放在此文件夹新建的一个```__init__.py```文件之中，这样就可以直接找到它们，不用再指定包具体的文件名。

### 07.12

> ***今天的问题是，tf官方的im2txt，还是有错。应该生成256个训练集的文件，但是我只生成了8个。有人说是tf版本和cuda的不匹配，但是我的应该是匹配的，很奇怪。***

### 07.16

> ***好久没写笔记。昨天解决了上面说的训练集生成不对的问题，开始训练***

TFRecords文件生成不对不是cuda版本问题，而是python2和pyhton3版本处理bytes和str不一样，因此会报关于UTF—8的错误。这个需要将读文件的命令从r改为rb（binary）,然后再将代码里改一句，就跑过了。剩下没什么大问题。
其实归根结底，tf官方这个代码，文档里说是用python2（可能对3也兼容），不过因为我不太会在用bazel的地方显示指定python2跑，而默认参数可能是3，所以就用3跑的。
一个小问题是训练很慢。平常我可以根据改变batch_size来提升GPU使用率，不过这个我改了以后，每一个迭代时间按比例增加，GPU使用率完全不变，这跟我以前遇到的情况也不太一样。官方给的是1,000,000个迭代，我这个硬件100多个小时。现在才过了十几个小时，目测还在收敛一点，我想知道能一直收敛到一周后？

|Time|Model|Epoch|Batch_size|lr|Loss|
|--|---|----|---|---|----|
|07.16|InceptionV3|136,000|32||2.300|

### 07.17

> ***又是瓶颈，昨天好像没有用GPU训练***

现在发现loss不下降，而且环境有有问题了。为了用GPU改了tf1.12好像还是不行。现在连指令都跑不起来。接下来试一下换回14（可以）

### 07.18

> ***有进展，tf官方的im2txt还可以***

很明显这个模型对于认识的目标，效果还可以，不认识的话，效果很差。所以训练自己的东西自己用。改了一个数组bug之后，自己的训练结果可以用，大概是350,000次迭代。别人的我还没用，理论上效果不能比我这个差，他们训练的都是1,000,000往上走，还有cnn的调优。之后看一下不能用gpu训练的问题，以及找点图片