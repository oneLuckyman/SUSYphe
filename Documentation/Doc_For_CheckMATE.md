# CheckMate 上手指南

CheckMate 是一个针对特定事例群模拟 LHC 限制的 C++ 程序。

> **<前提>：** MadGraph5_aMC@NLO 需要在 Linux 系统或 Mac OS 系统上运行。

如果已经按照安装指南进行了正确的安装并完成了测试，那么只需要准备一个文本格式的配置文件即可运行 CheckMATE。

下面是一个配置文件的示例：

```text
[Parameters]
Name: My_New_Run
Analyses: ams_sus_16_039, cms_sus_16_048
QuietMode: True
OutputExists: overwrite
RandomSeeb: 10

[myprocess]
XSect: 1 FB
Events: Path/to/your/hep/or/hepmc/file
```

这个文件中两个部分的功能分别是设定参数以及设定过程。

参数部分包括：

1. Name：指定本次运行的名称
2. Analyses：指定本次运行考虑哪些实验分析，只能指定 CheckMATE 内已经纳入的分析。
3. QuietMode：可选项。若设定为 True，则运行时 CheckMATE 将不再询问信息确认。
4. OutputExists：可选项。若指定的 Name 曾被使用过并且结果仍保留在结果文件夹内，那么运行时会被提示输入 overwrite(o)、add(a)、stop(s) 之一以继续。overwrite 表示覆盖已有结果，add 表示在已有结果上进行叠加、stop 表示停止运行。如果在此项输入这三者之一，运行时遇到该情况时将默认执行此项的指定值。
5. RandomSeed：随机数种子。

过程部分包括：

1. XSect：事例过程对应的截面值。
2. Events：事例文件的路径。

运行时需进入 CheckMATE 主目录下的 `./bin` 文件夹内，假设配置文件位于当前目录且名称为 `test.dat`，那么运行指令为：

```bash
./CheckMATE test.dat
```

运行的结果存放在 CheckMATE 主目录下的 `./results` 文件夹内，名称为配置文件中的 `Name` 选项。
