# MadGraph5_aMC@NLO 快速上手指南

**作者：** 贾兴隆

MadGraph5_aMC@NLO 是一个高能物理工具框架，旨在提供 SM 和 BSM 唯象学所需的所有元素，例如截面计算，硬事例产生，matching 等，与 Delphes 以及 Pythia 结合还可以实现探测器模拟和强子化模拟。

MadGraph5_aMC@NLO 是一个复杂，庞大的开源软件，一次性地完全所有相关内容的编写十分困难，因此本文档暂时只涉及工作流中最必要的那些操作而且不会对原理方面做出过多的阐述。如果正在阅读本文档的读者对该软件中的某些部分有着自己独到的理解，那么欢迎对文档进行补充。

!!! note <前提>：MadGraph5_aMC@NLO 需要在 Linux 系统或 Mac OS 系统上运行。

> <提示>：
本文档假定读者已经具备 Linux 系统的基本知识，并已经掌握了其基本的使用方法，如果您几乎不了解 Linux 系统，那么建议您先简单了解一下 Linux 的基本知识和使用方法以便更轻松地阅读本文档。
如果您确实不怎么了解 Linux 系统也请不用过于担心，本文档会尽力将有关演示的过程阐述清楚，只要您能跟着做下来相信也会有所收获。
Mac OS 系统与 Linux 系统都属于类 Unix 系统，有很多相似之处。如果您对 Mac OS 系统非常了解的话，那么使用该系统也是可以的。
本文档将使用 Ubuntu 发行版以及本文档编写时最新的 MG5_aMC_v3.5.1 版本作为演示环境。

## 下载和安装

尽管 MadGraph5_aMC@NLO 是一个 `Python` 与 `Fortran` 及少部分 C++ 语言混编的程序，但 MadGraph5 官方已经提供了好了预编译版本供用户使用，可以在 <https://launchpad.net/mg5amcnlo> 处下载到。

需要注意的是，不同版本的 MadGraph5 对 `Python` 版本的要求也有差异，例如 MadGraph5_v2.X 通常要求 `Python2` 而 MadGraph5_v3.X 通常要求使用 `Python3`，更细节的版本要求需要根据具体的 MadGraph5 版本而定。对于一个使用 `Python` 语言的软件来说，`Python` 环境的配置是一个很重要的问题，不过本文档不会对此进行过多的赘述，详情请了解 `Anaconda` 或 `miniconda` 软件。

MadGraph5 对于 `Fortran` 编译器(gcc, gfortran)版本同样有一定的要求，请根据实际使用的软件版本决定您的 `Fortran` 编译器版本，通常来说 `Fortran` 编译器 的配置要比 `Python` 环境的配置简单的多。

下载得到的文件是一个 `.tar.gz` 后缀的文件，这需要使用 `tar` 工具进行解压，使用方法是通过终端来到压缩包文件所在的目录位置，或直接在相应位置处打开终端，然后输入以下命令即可解压版本号为 `X.X.X` 的 MadGraph5：

```bash
tar -zxf  MG5_aMC_vX.X.X.tar.gz
```

执行完该命令后 `MG5_aMC_vX.X.X` 就会被解压到当前目录下，如果您想解压到其它目录，例如 `$PATH` 目录下，则可以在此命令后添加 `-C $PATH` 参数。

```bash
tar -zxf  MG5_aMC_vX.X.X.tar.gz -C $PATH
```

现在您已经可以使用 MadGraph5_aMC@NLO 了。

## 产生您的第一个粒子物理硬散射过程

首先，本文档将带您实际上手一个产生过程的示例，以便使您对 MadGraph5 的功能有一个基本的了解。

### 粒子物理过程产生

狭义上的 MadGraph 指的是粒子物理过程产生器，即，输入一个粒子物理过程，MadGraph 会产生相应过程可能的费曼图及对应的散射振幅。

进入解压后的 `MG5_aMC_vX_X_X` 目录，该目录下有一个 `bin` 文件夹，进入该文件夹内，运行以下命令即可打开一个形如 `Python` 交互界面的 MadGraph5_aMC@NLO 交互界面：

```bash
./mg5_aMC
```

!!! note `bin` 的意思是 `binary`，也就是二进制文件的含义。所有静态语言（例如 C++，Fortran）编写的程序都需要进行编译，而编译所产生的文件就是二进制文件，执行该文件就是在执行程序，所以这种文件也被称为可执行文件。但 `mg5_aMC` 文件并不是一个二进制文件而是一个 Python 文件，而且 Python 并不是静态语言，无需编译，也无法生成二进制文件。之所以这样的文件也被放在 `bin` 里，是因为 Python 文件可以在第一行通过 `#! */*/* python*` 语法指定默认 Python 解释器从而使该文件可以被直接执行，自动被默认解释器解释。因此，该文件在形式上和可执行文件没有什么区别，这也表明 `mg5_aMC` 这个文件可以通过 `python mg5_aMC` 这样的方式执行。

进入交互界面后，可以看到很多配置信息和提示信息，可以暂时先按下不表。此时已经可以与 MG5 进行交互了，输入以下命令以产生一个“质子—质子 → top 夸克—反 top 夸克” 过程的相关文件：

```MG5
MG5_aMC>generate p p > t t~
```

!!! note MadGraph5 的交互界面支持 Tab 键补全操作。

在得到一系列过程产生的提示信息后，界面会再次进入可交互状态，此时该过程相关的费曼图已经产生，接下来可以使用以下命令将文件导出：

```MG5
output Example
```

此时，带有 `p p > t t~` 过程相关文件的文件夹 `Example` 就已经在当前目录生成了，如果想在其它目录中导出，可以输入诸如以下命令：

```MG5
output ../Example
```

这样就会在当前目录的上一级目录中导出 `Example` 文件夹。

至此，狭义上的 MadGraph 流程就结束了。

### 事例产生器

产生完过程之后，就要运行事例产生器 Madevent，事例产生器会产生事例模拟，同时计算相应过程的截面，一共有三种事例产生方式。

#### 直接于 MadGraph 交互界面启动事例产生器

导出 `Example` 文件夹后，不要急于退出，继续输入 `launch` 以进入事例产生设置界面。

首先会进入一个功能开关及配置界面，这个界面包括以下内容：

1. 选择进行部分子雨和强子化模拟的程序，需要提前在交互界面输入 `install pythia8` 安装相关程序才可打开
2. 选择进行探测器模拟的程序，需要提前在交互界面输入 `install Delphes` 安装相关程序才可打开
3. 选择使用什么分析包，需要提前在交互界面输入 `install MadAnalysis4(/5)` 安装相关程序才可打开
4. 选择是否使用 madspin
5. 选择是否启用 reweight

在交互界面输入对应的数字以进行选择，完成后输入 `0` 或者 `done` 或者直接按回车以结束此部分配置。

接下来会进入 `Cards` 设置界面，输入对应的数字以打开特定的卡片编辑界面，默认情况下将使用 vi 编辑器打开：

1. param：设置参数卡片 param_card.dat。这是一个 SLHA 格式文件，应当包含所有必要的模型参数。无论如何都会有此选项。
2. run：设置运行卡片 run_card.dat，这是用来设置运行参数的文件。无论如何都会有此选项。
3. pythia8：设置 pythia8_card.dat 卡片。当在上一部分中打开部分子雨和强子化模拟功能时才会出现此项设置。
4. delphes：设置 delphes_card.dat 卡片。当在上一部分中打开探测器模拟功能时才会出现此项设置。
5. madspin：设置 madspin_card.dat 卡片。当在上一部分中选择使用 madspin 时才会出现此项设置。
6. reweight：设置 reweight_card.dat 卡片。当在上一部分中选择使用 reweight 时才会出现此设置。
7. madanalysis4(/5)_parton：设置 madanalysis4(/5)_parton_card.dat。当在上一部分中选择使用分析报告时才会出现此项设置。
8. madanalysis4(/5)_hadron：设置 madanalysis4(/5)_hardon_card.dat。当在上一部分中选择使用分析报告时才会出现此项设置。

完成您想要的配置后，输入 `0` 或者 `done` 或者直接按回车以结束此部分配置。

此时 MadGraph5 就会开始产生模拟事例，通常来说 pythia8 是最耗时的一个步骤。如果您的计算机具有图形界面以及浏览器，则运行后会弹出一个显示相关结果的网页。

#### 从导出文件进入交互界面运行事例产生器

在导出 `Example` 文件夹后，退出当前的交互界面并前往导出的 `Example` 文件夹中，文件夹所在的具体位置取决于导出时指定的目录位置。

在进入 `Example` 文件夹后，进入 `bin` 文件夹，这里有几个文件和一个文件夹，其中一个的名字是 `generate_events`。这是一个 `Python` 脚本，同样指定了默认解释器，因此可以通过 `./generate_events` 命令直接使用，打开后就会进入[上一节](#直接于 MadGraph 交互界面启动事例产生器)中输入 `launch` 后进入的界面，后续的使用方法和[上一节](#直接于 MadGraph 交互界面启动事例产生器)完全一样

#### 使用导出文件中的脚本，不进入交互界面直接运行事例产生器

在导出 `Example` 文件夹后，退出当前的交互界面并前往导出的 `Example` 文件夹中，文件夹所在的具体位置取决于导出时指定的目录位置。

在进入 `Example` 文件夹后，进入 `bin` 文件夹，这里有几个文件和一个文件夹，其中一个的名字是 `generate_events`。这是一个 `Python` 脚本，同样指定了默认解释器，因此可以使用 `./generate_events` 命令直接使用，如果加上 `-f` 参数，就可以跳过交互界面直接运行事例产生器。

```bash
./generate_events -f
```

但若是这样运行便无法使用交互界面对 `Cards` 进行修改。因此，要想修改 `Cards` 就需要进入到 `Example/Cards/` 文件夹内，直接对具体的 `Cards` 文件进行编辑修改。

## 结果文件

当事例产生器运行结束后，就可以在 `Example/Evevts` 文件夹中找到运行结果。首次运行结果存放在 `run_01` 中，如果再次运行该过程的事例产生器，那么结果会存放在 `run_02` 文件夹中，以此类推。以下讨论到的结果文件均位于这类文件夹内。

### lhe 文件

在[上一章](#产生您的第一个粒子物理硬散射过程)中，假设只使用最基础的事例产生器，即不使用强子化模拟和探测器模拟等额外组件，那么会直接产生一个 `unweighted_events.lhe.gz` 压缩文件。

使用 `gunzip unweighted_events.lhe.gz` 命令以解压此文件，解压完成后会得到一个名为 `unweighted_events.lhe` 的文本文件，该文件内存储着所有产生事例的相关信息。

### hepmc 文件

如果在[上一章](#产生您的第一个粒子物理硬散射过程)中，您打开了强子化模拟功能，那么输出文件会额外产生一个名为 `tag_1_pythia_events.hepmc.gz` 的压缩文件（以及一些其他包含次要信息的文件）。

### Delphes 输出