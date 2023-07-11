# d2l

> @**学习笔记**
>
> @资料来源：**Github: ** [d2l-ai](https://github.com/d2l-ai/d2l-zh) 	**Web：**[动手学深度学习](https://zh.d2l.ai/index.html)





## 环境搭建

> 配置⼀个环境来运⾏ Python、Jupyter Notebook、相关库以及运⾏资料中所需的代码



### 安装 miniconda

**搜索官网** 

通过搜索引擎 检索关键字 `miniconda` ，选择官网进入

**选择版本**

根据实际配置选择版本

ep：（我的电脑是windows系统）

找到Windows installers，

| Python version | Name                      | Size     |
| -------------- | ------------------------- | -------- |
| Python 3.10    | Miniconda3 Windows 64-bit | 53.9 MiB |
| Python 3.9     | Miniconda3 Windows 64-bit | 53.7 MiB |
|                | Miniconda3 Windows 32-bit | 67.8 MiB |
| Python 3.8     | Miniconda3 Windows 64-bit | 53.1 MiB |
|                | Miniconda3 Windows 32-bit | 66.8 MiB |
| ……             | ……                        | ……       |

因为之前安装过`Python 3.10`，最后我下载的就是适配`Python 3.10` 的 `Miniconda3 Windows 64-bit`

即`Windows` +`Python 3.10` >>> `Miniconda3 Windows 64-bit`

> 建议：选择 **不超过** `python 3.7` 的 `miniconda`版本

**安装**

根据指引一步一步安装即可

**配置环境变量**

```
D:\miniconda3 # 安装 miniconda时配置的安装路径
D:\miniconda3\Scripts
D:\miniconda3\Library\bin
```

**检查**

cmd 调出命令行窗口

执行`conda –V` 和 `python –V`

看见返回的版本描述即安装成功

**初始化**

`conda init`





### 配置虚拟环境

**创建虚拟环境**

书中例句：`conda create --name d2l python=3.9 -y`

> d2l，是自定义的环境名称； 
>
> python=3.9，是指安装`python3.9`  
>
>  建议：python版本**不要超过3.7**  

**激活环境**

书中例句：`conda activate d2l`

> 即`conda activate` + 环境名

**安装深度学习框架**

根据个人电脑配置，选择适配的版本安装（ GPU版本 和 CPU版本）

ep：我的电脑

> CPU：Intel(R) Celeron(R) CPU J3455 @ 1.50GHz
>
> GPU：Intel(R) HD Graphics 500 核显 

所以 安装 CPU版本

在环境（d2l）内，`pip install mxnet==1.7.0.post1`

**安装d2l软件包**

在环境（d2l）内，`pip install d2l==0.17.6`





### 下载学习资料

**D2L Notebook**

获取方式见本文开头申明

配合 `jupyter notebook ` 使用效果更佳





### jupyter notebook 

**添加 kernel** 

> 将 `核` 加入到 `jupyter notebook` 中使用，需要使用 `ipykernel`

ep：将 创建的虚拟环境 `d2l`，作为 `核` 加入到 `jupyter notebook` 中使用

```bash
# (d2l) 环境下
pip install ipykernel

jupyter kernelspec list # 查看 核
jupyter kernelspec remove [核名称] # 移除 核

# (d2l) 环境下
python -m ipykernel install --user --name [环境名称] --display-name [自定义名字]
```





### 运行入门demo

input：

```python
from mxnet import np, npx
npx.set_np()
x = np.arange(12)
x
```

output：

```python
array([ 0., 1., 2., 3., 4., 5., 6., 7., 8., 9., 10., 11.])
```

> 环境搭建成功，内容如下：
>
> - **miniconda**，`Windows` +`Python 3.10` >>> `Miniconda3 Windows 64-bit`
> - **python**，`python==3.7`
> - **mxnet**, `mxnet==1.7.0.post1`
> - **d2l**, `d2l==0.17.6`





### 遇到的阻碍和解决

出门装

- **miniconda**，`Windows` +`Python 3.10` >>> `Miniconda3 Windows 64-bit`
- **python**，`python==3.10`



**#1** 初会 mxnet 和 d2l

安装 `mxnet==1.7.0.post1` 失败，大片报错涌现，占据命令行界面。

转头 安装 `d2l==0.17.6`再败，

急急急，莫非要“三而竭”？搬救兵！！！ 

百度，启动！

“版本不匹配……”、“python3.10降低到……”

从版本不匹配入手，尝试安装低版本 mxnet 和 d2l，经测试最高稳定在  `mxnet==1.2.0`，`d2l=0.14.0`

运行测试代码，

```
File D:\miniconda3\envs\d2l\lib\site-packages\mxnet\gluon\data\dataloader.py:278
    generator = lambda: [(yield self._batchify_fn([self._dataset[idx] for idx in batch]))
                        ^
SyntaxError: 'yield' inside list comprehension
```

> 低版本中不支持或不能解析这种语法。



**#2** 关注 python版本

从**版本不匹配**思路出发，涉及 **python**、 **mxnet**、**d2l**的版本。由于测试代码中未使用到 **d2l**库，故可先**排除d2l**。

排查 **python**，

因为起初电脑上已有 `python3.10` ,所以为保持一致，我在创建**虚拟环境**时配置的依然是 `python3.10`，并不是书中示例 `python3.9`（虽然知道此处**虚拟环境**的python3.10和**电脑**的python3.10无关）

经了解 `python3.10`存在一些问题：python3自 `python3.10`版本后对 `requests`库有进行调整，`collections`中不能直接调用`Mapping`、`MutableMapping `。即在 `import` 时，`import collections.abc.Mapping/MutableMapping`。

结合 “python版本不宜过高”之建议和“python3.10降低到……版本……”之解决方案，认识到 python版本的重要性。

于是更新**装备**，`python==3.10` >>> `python==3.9`

测试代码，运行！！！

相同报错 =_=

> 虽然尚不能确认 python版本的嫌疑，考虑到 `python3.9`与书中一致，故 **python版本**嫌疑下降
>
> 跟进 d2l版本，发现d2l版本可以稳定到 0.17.0，虽不知为何但也不必深究
>
> 莫非真要应在 mxnet上？



**#3** 陷入僵局

坚持**版本不匹配**的思路，如果 **mxnet**版本过低而不支持某种语法，导致运行时报语法错误，是说的通的。

尝试安装 `mxnet==1.7.0.post1`，

在报错信息中，发现安装依赖 `numpy`库时发生中断，导致安装 `mxnet==1.7.0.post1`失败。

百度，启动！

“可能是读写权限不够……”、“降低python版本……”、“手动下载轮子本地安装……”

读写权限？经测试非此原因，排除。

至于 python版本，已经从`3.10`降低到`3.9`，而且**示例**中使用也是`3.9`，故排除。

本地安装？按照以往经验，一般在 `pip install xxx`时不太好用才会使用本地安装，使用 `pip install numpy `安装是正常的、成功的

> 麻了QAQ



**#4** 端倪初现

尝试按照 **mxnet** 依赖的 **numpy**版本要求(>= 1.8.2)，使用 `pip`先安装 **numpy**，

`conda list` 查看配置，看到有 **numpy** ，即安装成功，

再安装 `mxnet==1.7.0.post1`，

报错，依旧……

再看 `pip install mxnet==1.7.0.post1`执行日志，先安装 mxnet，然后安装相关依赖库，然后安装 numpy时中断，最终导致失败。

虚拟环境里已经有了 **numpy**，为什么还要下载 wheel和安装 ？为什么不检查环境中 numpy 是否存在？

好吧，这些是无力改变的，执行程序嘛，接受就完了

好像又陷入僵局

再次仔细浏览报错信息，终于让我从一直忽视的版本号发现了端倪

```
  Collecting numpy<1.17.0,>=1.8.2
    Using cached numpy-1.16.6.zip (5.1 MB)
    Preparing metadata (setup.py) ... done
```

在安装  `mxnet==1.7.0.post1`时，指定 `numpy==1.17.0` ，但是它在安装 numpy-1.16.6.zip！！！

> 矛盾即问题所在





**#5**  真相大白

经过前面的实验，我了解到在安装 mxnet中安装该依赖 numpy时的“强制性”，体现在即使环境内先安装对应版本的 numpy，并不会因为numpy已经存在就跳过执行，而是依然执行安装步骤。

受本地安装的启发，于是到 pip官网看了 `numpy==1.16.0`

惊奇地发现它最高只有到**python37**的包！！！

此刻答案终于浮出水面，确实是版本不匹配

即 `mxnet==1.7.0.post1`依赖 `numpy=1.16.6` ，而 `numpy=1.16.6` 最高支持 `python=3.7`。

我现在是 `python3.9`，会报错没毛病嘛，合情合理好吧

“python版本不宜过高”之建议、“版本匹配”之坑 ，初听无感，如今默然

真相大白，版本不匹配、降低python版本……指的是python版本对库不同版本的支持和应用方式，这才是版本与版本匹配的真相。

最后我降到 `python3.7`后，**mxnet**可以正常安装，**d2l**也可以正常安装，运行案例也可以正常运行。

> 真不戳啊 ~





**#6**  回顾和思考

满装备

> - **miniconda**，`Windows` +`Python 3.10` >>> `Miniconda3 Windows 64-bit`
> - **python**，`python==3.7`
> - **mxnet**, `mxnet==1.7.0.post1`
> - **d2l**, `d2l==0.17.6`

波折

- 以“本地安装——解决 pip安装异常”的经验先入为主，幸查看了 `numpy==1.16.6`安装包的最高支持版本而破除版本匹配之谜

- python版本一降再降，`python3.10` >>> `python3.9` >>> `python3.7` 。**3.10** 对 `requests`库有进行调整，运行中多有不异常故而舍之；**3.9**  因为与书中示例一致，先入为主故不疑有他，直至 `numpy==1.16.6` 惊觉，最终降为 **3.7**

存疑

- 先安装 **mxnet**后安装 **d2l**，发现 **numpy**在安装 `d2l== 0.17.6`后，经历过卸载和高版本的安装。由此可以得知 `numpy== 1.16.6`并没有我想象的那么重要，仅仅是 `mxnet==1.7.0.post1`所需
- 成功实现也有可能是 **python3.7** 的特性。常见之建议，python版本不宜过高，不外如是。
- GPU版才是主流和主力，出现波折也许是因为CPU版本的 mxnet的版本跟进太慢

