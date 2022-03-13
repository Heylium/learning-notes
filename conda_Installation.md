#### conda，Rdkit安装及PyCharm配置

#### 一、下载安装Anaconda

##### 1.下载Anaconda

在官网https://www.anaconda.com/下载，或选择清华镜像https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/ 

##### 2.安装Anaconda

普通情况，打开下载安装包一路next即可。

若要自定义安装，打开安装包后：

Installation Type建议选择all users

![image-20220312230416437](C:\Users\Xiang_Duan\AppData\Roaming\Typora\typora-user-images\image-20220312230416437.png)

Installation Location选择你想要安装到的目录

![image-20220312230656087](C:\Users\Xiang_Duan\AppData\Roaming\Typora\typora-user-images\image-20220312230656087.png)

Installation Option：Add to Path 官方建议从开始菜单来启动Anaconda，此处我为了以后方便勾选了，Register Anaconda选项勾选后系统调用会优先使用Anaconda自带的Python。

![image-20220312230927004](C:\Users\Xiang_Duan\AppData\Roaming\Typora\typora-user-images\image-20220312230927004.png)

之后正常Install即可。

##### 3.配置Anaconda

安装完成后，可以进到Anaconda的安装目录（例子中是D:\Anaconda）,在目录中查看是否有conda.exe文件（或 _conda.exe，为了方便后续使用，此处删除 _conda.exe文件名前的短下划线“\_"）。

此时，在Windows开始菜单栏的应用列表中，应该能看到Anaconda及其附带应用

![image-20220312232401884](C:\Users\Xiang_Duan\AppData\Roaming\Typora\typora-user-images\image-20220312232401884.png)

点击Anaconda Navigator，进入conda的图形化管理界面，如果一直卡在initialization界面，可以考虑完全清理Anaconda的文件后后重装。或者进入cmd命令行输入

```
tasklist | findstr "pythonw"
```

查找conda界面的进程，如12345，而后输入

```
taskkill -pid <进程id> -f	//taskkill -pid 12345 -f
```

kill界面的进程后尝试重新载入。

#### 4.修改Anaconda仓库为清华源

在用户目录下查看是否有.condarc文件，如果没有，则可在Anaconda Promote中输入

```
conda config --set show_channel_urls yes
```

应当能在C:\Users\Administrators下找到.condarc文件，以记事本方式打开，贴入以下url保存即可

```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```



#### 5.配置conda工作环境

base环境在Anaconda安装目录下，为了方便后续操作，我将base环境复制一份到工作目录下，并创建一个虚拟环境。

例如，我想在D:\Programmer\Python\Anaconda\conda_files\envs目录下创建一个名为baseclone的conda 虚拟环境，这个环境的配置由base环境clone而来。

在开始菜单栏的Anaconda目录下，找到Anaconda Promote，点击进入base环境的命令行交互界面，输入

```
conda create --prefix=D:\Programmer\Python\Anaconda\conda_files\envs\baseclone --clone base
//创建虚拟环境后需要激活才能使用
conda activate baseclone


//普通安装直接先输入
conda create your_env_name
//然后
conda activate your_env_name


//想要查看已创建的所有虚拟环境，可以输入
conda env list
```

对于已创建的环境，可以通过在conda activate env_name 来激活并进入环境，如果出现

“CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.”错误，可以尝试在Anaconda Promote命令行输入

```
conda.bat activate env_path
//例如
conda.bat activate D:\Programmer\Python\Anaconda\conda_files\envs\baseclone
```

#### 二、下载Rdkit

在以上的虚拟环境的命令行中（例如我创建的baseclone），可以直接输入以下命令安装Rdkit包

```
conda install rdkit
//如果不成功，可以使用以下命令：
conda install -c conda-forge rdkit
```

如果想通过本地安装包安装rdkit，可以在rdkit网址https://anaconda.org/rdkit/rdkit下载正确的版本，比如[win-64/rdkit-2020.09.1.0-py37h3d1ada6_1.tar.bz2](https://anaconda.org/rdkit/rdkit/2020.09.1.0/download/win-64/rdkit-2020.09.1.0-py37h3d1ada6_1.tar.bz2)，然后将下载好的文件放置在Anaconda安装目录的pkgs子目录下（如D:\Programmer\Python\Anaconda\conda_files\pkgs），而后在命令行输入（建议在想要安装rdkit的虚拟环境的Anaconda Promote命令行）

```
conda install --use-local package_name
//例如
conda install --use-local D:\Programmer\Python\Anaconda\conda_files\pkgsrdkit-2020.09.1.0-py37h3d1ada6_1.tar.bz2
```

#### 三、配置Pycharm

打开Pycharm，新键一个Project，在项目类型中，选择Pure Python。在Python Interpreter中，选择Conda，选择对应的python版本即可。

或者，在之前的例子中，我们已经创建好了一个conda虚拟环境baseclone，因此可以直接使用，随便创建一个python工程，File-Settings-Project:<project_name>-Python Interpreter中，点击Python Interpreter栏后的设置标签，选择Add。

![image-20220313004802263](C:\Users\Xiang_Duan\AppData\Roaming\Typora\typora-user-images\image-20220313004802263.png)

选择Conda Environment，然后选择Existing environment，解释器选择虚拟环境的python.exe，conda executable选择对应的conda.exe即可。

![image-20220313004908580](C:\Users\Xiang_Duan\AppData\Roaming\Typora\typora-user-images\image-20220313004908580.png)

配置完成后，可以在工程下新建一个python文件，import检验一下

```python
from rdkit import Chem
```

点击运行，如果不报错，则安装完成。

#### 四、后记

rdkit可以实现将smiles格式分子转换为图片输出，在python文件中输入以下代码

```python
from rdkit import Chem
from rdkit.Chem import AllChem
from rdkit.Chem import Draw
# from rdkit.Chem.Draw import IPythonConsole #Needed to show molecules
from rdkit.Chem.Draw.MolDrawing import MolDrawing, DrawingOptions  # Only needed if modifying defaults
import os

smis = [
    'COC1=C(C=CC(=C1)NS(=O)(=O)C)C2=CN=CN3C2=CC=C3',
    'C1=CC2=C(C(=C1)C3=CN=CN4C3=CC=C4)ON=C2C5=CC=C(C=C5)F',
    'COC(=O)C1=CC2=CC=CN2C=N1',
    'C1=C2C=C(N=CN2C(=C1)Cl)C(=O)O',
]
mols = []
for smi in smis:
    mol = Chem.MolFromSmiles(smi)
    mols.append(mol)

img = Draw.MolsToGridImage(
    mols,
    molsPerRow=4,
    subImgSize=(200, 200),
    legends=['' for x in mols]
)
work_root = os.getcwd()

img.save(work_root + '\mol11.jpg')

```

点击运行，应该能在当前Project目录下找到一个名为mol11.jpg，的图片。

![mol11](D:\mol11.jpg)

