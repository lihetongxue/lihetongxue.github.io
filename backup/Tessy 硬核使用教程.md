## 0. Tessy 简介

> Tessy是一个专门针对嵌入式软件的C/C++代码进行单元、集成测试的工具，它可以自动化地执行测试、评估测试结果并生成测试报告

在以V模型为例的开发模式中，Tessy主要应用在单元/模块测试，集成/组件测试。在V模型的开发模式中，单元测试是第一个动态测试活动。通过函数级测试，有效检查出每一类错误，比如算法错误、边界控制错误、数据溢出等。通过单元测试，可以尽可能早得发现Bug，防止到后面的测试过程或者到最终用户那里才被发现。在整个项目开发测试周期中，越早发现Bug越好，事实证明，开展详尽规范的单元测试能够有效减少系统测试和回归测试时间，降低开发成本

另外，Tessy也可以满足各类标准（ISO26262、IEC 61508、 EN 50128/50129、 DO-178B、汽车SPiCE或FDA的软件验证通用原则）对测试的需求，比如ISO26262中各个测试等级中对模块测试的要求可以使用Tessy来满足，当然Tessy本身也通过了Tuev的认证，可以在安全相关性的软件研发过程中被使用

## 1. 新建工程

* **启动Tessy，点击File选择New Project...新建工程**

![image](https://lihe.ink/2025/03/07/67ca6851cb3da.png)

* **在弹出的界面中输入工程名；Project Root中选择指定你的Tessy工程想要存放的路径；Source Root中勾选Enable source root，下面框中选择你的代码存放路径；其他选择默认就行；最后点击OK创建成功**

> [!IMPORTANT]
> 工程名以及相关路径不能包含中文字符和空格，否则报错

![image](https://lihe.ink/2025/03/07/67ca68cc0e9fe.png)

* **创建成功后，Tessy重启，首次会直接进入刚创建的工程，多个工程切换可以点击File选择Select Project选中你需要打开的工程**

![image](https://lihe.ink/2025/03/07/67ca69372f026.png)

![image](https://lihe.ink/2025/03/07/67ca6959d3eb0.png)

## 2. 导入源代码

* **依次点击红框中三个图标，分别对应新建测试集、新建文件夹、新建模块；建议有规划的根据你的代码层级创建且名称具有意义**

![image](https://lihe.ink/2025/03/07/67ca697b47dff.png)

![image](https://lihe.ink/2025/03/07/67ca69989740b.png)

* **选中新建的模块，General配置如下，一般默认即可**

![image](https://lihe.ink/2025/03/07/67ca69b3bfbd4.png)

* **Source配置如下，添加需要测试的模块；添加头文件目录**

> [!TIP]
> 强烈建议将项目代码中所有头文件存放在一个目录中，例如都存放在HHH目录中；此做法可避免大量的报错

![image](https://lihe.ink/2025/03/07/67ca69cf6de87.png)

* **Coverage配置如下；点击红框部分选择覆盖类型，如语句覆盖ASIL B；接着点击Perferences配置如下图**

![image](https://lihe.ink/2025/03/07/67ca69fa4ee3e.png)

![image](https://lihe.ink/2025/03/07/67ca6a11ef940.png)

* **点击下图其中一个红框进行编译分析**

![image](https://lihe.ink/2025/03/07/67ca6a392ba15.png)

* **待分析结束后，如果分析过程中出现报错，则需要进入控制台（软件右下角）进行问题分析并解决；再次进行编译分析直至成功分析完成**

> [!NOTE]
> 控制台中报error部分问题需要解决，warning部分可忽略不做处理，对工程无影响

![image](https://lihe.ink/2025/03/07/67ca6a5a62261.png)

* **如果编译分析无问题，则分析成功，模块的下拉列表会显示该模块中所有的函数**

![image](https://lihe.ink/2025/03/07/67ca6a7639e8a.png)

* **小概率Tessy会生成一些自带的函数，需要屏蔽它们（图标变灰），如下图**

![image](https://lihe.ink/2025/03/07/67ca6a9587ab4.png)

![image](https://lihe.ink/2025/03/07/67ca6ab885dde.png)

## 3. 界面介绍

> 一共有9个界面，每个界面对应了不同的工作区，如下图

![image](https://lihe.ink/2025/03/07/67ca6ad603599.png)

1. **Requirement Mangement：** 需求管理界面，目前不适用

2. **Coverage Viewer：** 代码覆盖情况查看界面

3. **Overview：** 全局界面

4. **Test Interface Editor：** 接口配置界面

5. **Classification Tree Editor：** 分类树界面（用于集成测试）

6. **Test Data Editor：** 用例设计界面

7. **Scenario editor：** 工具场景界面，目前不适用

8. **Script Editor：** 脚本编辑界面

9. **Interface Data Assigner：** 接口数据管理界面（用于回归测试及代码修改后）

## 4. 接口配置处理

> 点击进入Test Interface Editor(TIE)界面，对函数接口进行配置，该界面会显示该函数的所有输入输出及函数接口

![image](https://lihe.ink/2025/03/07/67ca6afcc617b.png)

* **函数打桩；打桩后函数接口图标由空心变成蓝色实心**

> [!IMPORTANT]
> 1. 接口中所有函数都必须进行打桩，包括内部和外部所有的函数，并且未使用部分的External Functions中的函数也必须全部打桩，否则执行用例将会报错
> 2. 桩函数分为普通桩和高级桩，如果有高级桩的选项，必须打高级桩，只有普通桩则就打普通桩，否则执行用例时将报错


* **变量接口处理；根据代码逻辑对变量接口进行配置为输入输出等**

> [!TIP]
> 默认选择的接口配置有些会有出入，需要根据代码实际修改；一些不影响覆盖率的变量可以设置为不关心

![image](https://lihe.ink/2025/03/07/67ca6b6803e47.png)

* **按Ctrl+S保存你的配置，或者点击其他界面时会提醒你是否保存**

## 5. 设计测试用例

> 点击Test Data Editor(TDE)界面，即可进行用例设计

* **新建测试用例**

![image](https://lihe.ink/2025/03/07/67ca6beeeb589.png)

* **设计测试用例**

> [!TIP]
> 1. 用例设计应当结合源代码进行设计；用例步骤可单独执行测试，也可多步骤或多用例一起执行
> 2. 每条用例或步骤可整体复制粘贴，可以节省用例设计时间
> 3. 每条用例填写完后必须手动Ctrl+S进行保存，图标将由灰色变为黄色

![image](https://lihe.ink/2025/03/07/67ca6c0c42259.png)

* **执行测试用例；如图Ctrl+A选中所有需要执行的用例后，点击红框中的绿色三角形执行测试**

![image](https://lihe.ink/2025/03/07/67ca6c2fc3c94.png)

* **输出与实际值；如果输出和预期值不一致，则需要改成实际值，可一键更改**

> [!NOTE]
> 由于可以一键更改实际值，我们设计用例时可只设计输入，输出则可为任意值

![image](https://lihe.ink/2025/03/07/67ca6c4ee8450.png)

![image](https://lihe.ink/2025/03/07/67ca6c7235c23.png)

* **查看覆盖度情况**

> 点击Coverage Viewer(CV)界面查看覆盖情况
>
> 点击流程图中的任意节点和路径即可看到源代码及覆盖情况

> [!TIP]
> 在设计用例时，利用这个界面了解未覆盖部分情况可代码，能够大大提高测试效率

![image](https://lihe.ink/2025/03/07/67ca6c99e5ded.png)

## 6. 生成报告

> **按照下图生成并导出测试报告**

> [!NOTE]
> 报告只选取PDF文件，其中包含一个Overview report和若干个Detail report

![image](https://lihe.ink/2025/03/07/67ca6cd15ed3a.png)

![image](https://lihe.ink/2025/03/07/67ca6d01d955c.png)

* **Overview report；报告总览**

https://qtiu5p27dc.feishu.cn/drive/folder/CXyUfKKumlxApcd4oAZcqrTTnoc?from=from\_copylink

* **Detail report；详细报告**

https://qtiu5p27dc.feishu.cn/drive/folder/W0RUfsgRIldNYIdgsdcc7IylnMd?from=from\_copylink

## 7. 问题处理与操作

> 导入工程后，在编译分析阶段、用例执行阶段两个阶段过程中，每个项目都大概率会出现一些问题；然而花费时间的地方正是这些地方，所以针对某个项目或者兄弟项目时，解决这些问题可以让我们一劳永逸；这里列出一些常见的问题类型，并提供解决思路

* **开始之前**

1. 在开始导入代码前，先将工程代码中所有的#error替换成//#error屏蔽掉

2. 将所有头文件提取并放在一个文件夹中

* **编译分析阶段**

1. 出现头文件找不到的情况，报错为No such file or directory xxx.h

这种情况大概率为头文件未包含或者重复包含，检查头文件是否存在，如果未包含则需要将头文件放置到报错的文件中（C文件或者调用的头文件）

* 出现变量找不到的情况，报错为某某变量未定义等 
                                                                                                                                                                                                          
这种情况就是头文件的问题，将变量源头所在的头文件找到并添加到测试的C代码文件中

* 出现\_\_builtin\_va\_end和\_\_builtin\_va\_start相关的底层编译器相关的报错

在Tessy环境配置中添加编译器自定义命令(谨慎操作)：\_\_builtin\_va\_start=;\_\_builtin\_va\_end(ap)=((void)0)&#x20;

* 出现其他情况等（还会遇到很多问题）

根据控制台报错信息针对处理，一条一条解决

* **执行用例阶段**

1. 出现Unable to rececive block size这个问题

该问题为指针引起，第一个原因是在设计测试用例过程中，未将指针变量设置为动态类型，第二个原因不可避免，这个时候将代码中包含指针的语句进行屏蔽，则可完美解决

* 出现Const变量作为输入时，会因为常量无法改变报错

这个问题有两种解决方法，第一种最简单的就是将const去掉暂时解决执行过程，第二种方法可以配置Tessy环境，将Const变量设置为可作为输入（谨慎操作）

* 并未出现语法上的报错，报错信息很少（不作举例）

这个时候大概率是由于忘记打桩或者没打高级桩，或者TIE界面中接口为Unknow

* 测试技巧

如果花费大量时间仍然没有解决一些问题，可以针对报错部分进行屏蔽，即哪里报错屏蔽哪里（前提不影响大的方向下）
