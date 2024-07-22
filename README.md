## VoxPoser: Composable 3D Value Maps for Robotic Manipulation with Language Models

#### [[Project Page]](https://voxposer.github.io/) [[Paper]](https://voxposer.github.io/voxposer.pdf) [[Video]](https://www.youtube.com/watch?v=Yvn4eR05A3M)

[Wenlong Huang](https://wenlong.page)1, [Chen Wang](https://www.chenwangjeremy.net/)1, [Ruohan Zhang](https://ai.stanford.edu/~zharu/)1, [Yunzhu Li](https://yunzhuli.github.io/)1,2, [Jiajun Wu](https://jiajunwu.com/)1, [Li Fei-Fei](https://profiles.stanford.edu/fei-fei-li)1

1Stanford University, 2University of Illinois Urbana-Champaign
这是VoxPoser的官方演示代码，VoxPoser是一种利用大型语言模型和视觉-语言模型来进行零-shot合成操纵任务轨迹的方法。

在这个代码库中，我们提供了[VoxPoser](https://voxposer.github.io/)在[RLBench](https://sites.google.com/view/rlbench)中的实现，因为它的任务多样性最接近我们的真实世界设置。请注意，VoxPoser是一种零-shot方法，不需要任何训练数据。因此，这个代码库的主要目的是提供一个演示实现，而不是一个评估基准。
![](media/teaser.gif#id=hnqv1&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=550)



如果您发现这项工作在您的研究中有用，请使用以下BibTeX进行引用：

```
@article{huang2023voxposer,
      title={VoxPoser: Composable 3D Value Maps for Robotic Manipulation with Language Models},
      author={Huang, Wenlong and Wang, Chen and Zhang, Ruohan and Li, Yunzhu and Wu, Jiajun and Fei-Fei, Li},
      journal={arXiv preprint arXiv:2307.05973},
      year={2023}
    }
```

## 开始介绍

请注意，最好在有显示器的情况下运行此代码库。如果要在无头模式下运行，请参考[RLBench中的说明](https://github.com/stepjam/RLBench#running-headless)。

- 创建  conda 环境:

```shell
conda create -n voxposer-env python=3.9
conda activate voxposer-env
```

- 查看[安装PyRep和RLBench的说明](https://github.com/stepjam/RLBench#install)（注意：在创建的conda环境中安装这些）。 
-  安装其他依赖项：
```shell
pip install -r requirements.txt
```

- 获取一个OpenAI API密钥，并将其放入演示笔记本的第一个单元格中。

## 运行demo
演示代码位于`src/playground.ipynb`。说明可以在笔记本中找到。
## 代码结构
VoxPoser的核心内容：

- **`playground.ipynb`**：VoxPoser的演示代码。
- **`LMP.py`**：语言模型程序（LMPs）的实现，用于递归生成代码以分解指令，并为每个子任务组合值映射。
- **`interfaces.py`**：为语言模型（即LMPs）在体素空间中操作并调用运动规划器提供必要的API接口。
- **`planners.py`**：贪婪规划器的实现，为给定值映射的实体/可移动物体规划轨迹（表示为一系列航点）。
- **`controllers.py`**：给定实体/可移动物体的航点，控制器应用（一系列）机器人动作来达到航点。
- **`dynamics_models.py`**：环境动力学模型，用于实体/可移动物体为对象或对象部分的情况。在`controllers.py`中用于执行MPC。
- **`prompts/rlbench`**：VoxPoser中不同语言模型程序（LMPs）使用的提示。

环境和工具：

- **`envs`**：
   - **`rlbench_env.py`**：RLBench环境的包装器，用于为VoxPoser暴露有用的函数。
   - **`task_object_names.json`**：VoxPoser暴露的对象名称及其对应的每个个体任务的场景对象名称的映射。
- **`configs/rlbench_config.yaml`**：RLBench环境中所有相关模块的配置文件。
- **`arguments.py`**：配置文件的参数解析器。
- **`LLM_cache.py`**：语言模型输出的缓存，将其写入磁盘以节省成本和时间。
- **`utils.py`**：实用函数。
- **`visualizers.py`**：基于Plotly的值映射和规划轨迹的可视化器。

## 复现流程

## 一、虚拟环境创建

- 创建  conda 环境:

```shell
conda create -n voxposer-env python=3.9
conda activate voxposer-env
```

- 查看[安装PyRep和RLBench的说明](https://github.com/stepjam/RLBench#install)（注意：在创建的conda环境中安装这些）。 (这一步参考 二、安装PyRep和RLbench)
-  安装其他依赖项：
```shell
pip install -r requirements.txt
```

- 获取一个OpenAI API密钥，并将其放入演示笔记本的第一个单元格中。

这里我选择国内中转openai的api。

## 二、安装PyRep和RLbench
### 1.安装PyRep
PyRep需要CoppeliaSim的4.1版本。下载地址：

- [Ubuntu 16.04](https://www.coppeliarobotics.com/files/V4_1_0/CoppeliaSim_Edu_V4_1_0_Ubuntu16_04.tar.xz)
- [Ubuntu 18.04](https://www.coppeliarobotics.com/files/V4_1_0/CoppeliaSim_Edu_V4_1_0_Ubuntu18_04.tar.xz)
- [Ubuntu 20.04](https://www.coppeliarobotics.com/files/V4_1_0/CoppeliaSim_Edu_V4_1_0_Ubuntu20_04.tar.xz)

经过尝试，ubuntu22.04可以使用ubuntu20.04的CoppeliaSim安装包。
然后下载之后，推荐放在home文件夹（主文件夹）下。

![屏幕截图 2024-04-07 11:33:05.png](https://cdn.nlark.com/yuque/0/2024/png/22340347/1712460809301-130e5e66-baa7-42d6-9fd6-a4421b3bae8f.png#averageHue=%23f3f0ee&clientId=u4d6e5b2c-b51b-4&from=drop&id=y7K1K&originHeight=529&originWidth=897&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70569&status=done&style=none&taskId=u8c55be6a-2d72-49c4-8dcd-2ba987d7833&title=)解压到这里即可，如图所示。
设置环境变量：

终端输入命令：
`sudo gedit ~/.bashrc # 打开环境变量`
然后加上内容：

```
export COPPELIASIM_ROOT=/home/sz/CoppeliaSim_Edu_V4_1_0_Ubuntu20_04
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$COPPELIASIM_ROOT
export QT_QPA_PLATFORM_PLUGIN_PATH=$COPPELIASIM_ROOT
```
第一行改成自己的安装路径。
然后：
`source ~/.bashrc`

**PyRep安装**

**按照github的readme，一步步来.**
[**https://github.com/stepjam/PyRep**](https://github.com/stepjam/PyRep)
**下面的我是在voxposer-main路径下执行的，所以下载到了voxposer文件夹下。（应该在任何路径都可以，注意安装的时候安装到虚拟环境中。）**
```
git clone https://github.com/stepjam/PyRep.git
cd PyRep
pip3 install -r requirements.txt
pip3 install .
python3 setup.py install --user
```




**这里，我后面发现python3 setup.py install --user将pyrep安装在base env里面了，不是我要用的虚拟环境**
**改为虚拟环境中，cd到pyrep下，**
```
conda activate voxposer-env
cd PyRep

```
**运行.**
```
pip install -e.
```
**运行下面代码验证**
```
python3 examples/example_baxter_pick_and_pass.py
```


### 2.RLBench安装

[https://github.com/stepjam/RLBench#install](https://github.com/stepjam/RLBench#install)

在虚拟环境中，安装步骤老三样：
```
git clone https://github.com/stepjam/RLBench.git
cd RLBench
pip install -r requirements.txt
pip install .
```

然后将RLBench添加进env
```
pip install -e. 
```


终端运行:
```
python3 examples/single_task_rl.py
```


### 补充-pycharm运行问题

pycharm识别不到环境变量：
所以报错：ImportError: libcoppeliaSim.so.1: cannot open shared object file: No such file or directory
简单来说就是给bashrc加个path，然后通过终端pycharm.sh启动，这样就可以pycharm识别到系统环境变量
解决方案：[https://blog.csdn.net/hehedadaq/article/details/86634040](https://blog.csdn.net/hehedadaq/article/details/86634040)
