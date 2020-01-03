@[toc]( gprmax 3.0 安装与cmd、pycharm（或jupyter）运行gprmax详细教程)

## 0.引言

本人最近也正学习gprmax仿真，在安装和使用过程中也遇到了许多问题，例如系统问题，cmd操作不便等等，希望我的一些经验能对你有一定的帮助。

在安装或使用过程中有什么问题可通过邮箱**1593458764@qq.com**联系我，希望能多相互交流学习，共同进步
## 1. 下载gprmax包

[点击此处进入下载](https://github.com/gprmax/gprMax)

## 2. 安装

### 2.1 安装anaconda（添加至环境变量）

 2.2.1 anaconda安装最后一步可选添加至环境变量和默认python解释器

 2.2.2  若安装anaconda是没选择添加至环境变量，可选择右键“我的电脑”>选择“属性”>高级系统设置>环境变量

在系统变量中找到path，点击编辑，将光标移到最后面，输入一个分号  ( ; )，将anaconda安装目录下scips文件夹的路径复制到分号后（注意使用英文输入法），确定，保存关闭，重启anaconda

### 2.2 更新conda

```cmd命令
conda update conda
```

### 2.3 安装git

```cmd命令
conda install git
```

### 2.3 安装gprmax所需包

```cmd命令
#导航至gprmax顶级目录，这里我是直接将gprmax文件夹放在桌面了，其他盘使用cmd命令导航至其所在目录即可
cd desktopcd
cd gprmax
conda env create -f conda_env.yml

```

### 2.5 安装microsoft visual c++2015

利用[microsoft visual c++2015 tool](http://download.microsoft.com/download/5/F/7/5F7ACAEB-8363-451F-9425-68A90F98B238/visualcppbuildtools_full.exe)进行安装，自定义安装，只选择安装与你Windows版本一致的SDK（Windows版本一般有两个SDK可选择，选与windows版本相符的即可，当然怕选错也可以直接安装完全版）

### 2.6 安装gprmax

```cmd命令
#激活目录
conda activate gprMax
#安装
python setup.py build
python setup.py install
```

## 3. 注意

以上安装步骤均在cmd命令窗口进行

安装完成后后续仿真过程中可能需激活gprmax目录


## 4. cmd命令运行gprmax
```cmd命令
#cmd命令窗口运行gprmax

cd desktop\gprmax  #导航至gprmax顶级目录
conda activate  gprmax  #激活gprmax目录
python -m gprMax user_models/inputfilename.in #A扫描模拟生成.out文件
python -m gprMax user_models/inputfilename.in -n 50 #B扫描模拟生成n个.out文件
python -m tools.plot_Ascan/outputfilename.out #A扫描绘图
python -m tools.plot_Ascan/my_outputfilename.out --outputs Ez -fft #绘制Ez分量及其fft图
python -m tools.plot_Bscan/outputfile_merged.out rx-component  #B扫描绘图
#多个A扫描.out文件合并成一个.out文件
python -m tools.outputfiles_merge user_models/outputfilebase
#多个A扫描.out文件合并成一个.out文件并删除单独A扫描的文件
python -m tools.outputfiles_merge user_models/outputfilebase --remove-files
```

## 5、pycharm运行gprmax
### 5.1 模型仿真
新建项目，解释器选择为gprmax对应的python解释器即可通过pycharm运行gprmax
```python

"""
python运行gprmax
读取.in文件
运行api函数模拟
"""
import os
from gprMax.gprMax import api
#文件路径+文件名
dmax=r"C:\Users\Desktop\gprMax" # gprmax install path
filename = os.path.join(dmax, 'user_models', 'concrete.in')
#正演  n：仿真次数（A扫描次数）->B扫描
api(filename, n=1, geometry_only=False) #geometry_only：仅几何图形
merge_files(r"C:\Users\Desktop\gprMax\concrete", removefiles=True)
```
### 5.2、 A扫描绘图
```python
"""A扫描"""
import os
from gprMax.receivers import Rx
from tools.plot_Ascan import mpl_plot
dmax=r"C:\Users\Desktop\gprMax" # gprmax install path
filename = os.path.join(dmax, 'user_models', 'concrete.out')
outputs = Rx.defaultoutputs
outputs = ['Ez']
plt = mpl_plot(filename,outputs, fft=True)
```



### 5.3、 B扫描及保存回波数据

```python
"""B扫描绘图"""
import os
import numpy
import numpy
from tools.plot_Bscan import get_output_data, mpl_plot
dmax=r"C:\Users\Desktop\gprMax" # gprmax install path
filename = os.path.join(dmax, 'user_models', 'concrete_merged.out')
rxnumber = 1
rxcomponent = 'Ez'
#获取回波数据
outputdata, dt = get_output_data(filename, rxnumber, rxcomponent)
#保存回波数据
numpy.savetxt('wavedata.txt',outputdata,delimiter=' ')
#绘图
plt = mpl_plot(filename,outputdata, dt, rxnumber, rxcomponent)
```
## 6、jupyter运行gprmax
jupyter运行gprmax代码与pycharm一样，安装gprmax时会自带安装jupyter