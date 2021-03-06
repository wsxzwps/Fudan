# 动物推断专家系统


本专家系统是基于规则的专家系统，实现的功能为：给出某种动物的特征描述，根据规则推断这种动物是猫、鱼、蛇、鸟中的哪一种。


## 事实库

事实库中的事实描述了某种动物的特征，这些特征包括生活环境、形貌特征、动物习性等等。根据这些事实，再加上规则，我们就可以推断出这种动物是哪种动物。

我们把事实写成文本，保存在`facts.txt`文件中，这个文件就代表一个事实库，示例事实库如下：

```
live on land
have legs
```

这个事实库包含两条事实，说明了该种动物的两个特征：生活在陆地上；有足。


## 规则库

规则库中包含一系列规则。每条规则包含前项（antecedent）部分和后项（consequent）部分，前项说明了得到结论所需的条件，后项说明了由条件能得到的结论。

规则的示例为：
```
IF		前项1
	and	前项2
THEN	后项
```

在本专家系统中，规则的前项一般是动物的特征，后项是关于动物种类的推断，规则的前项之间用逻辑关系and连接。

本系统的规则库保存在文件`rules.txt`中，共包含5条规则，示例如下：
```
IF		live in water
THEN	the animal is fish

IF		have feather
THEN	the animal is bird

IF		can fly
THEN	the animal is bird

IF		live on land
	and	have legs
THEN	the animal is cat

IF		live on land
	and	have no legs
THEN	the animal is snake
```


## 推理引擎

推理引擎的作用是根据事实库和规则库进行推理，推断出动物的种类（即输出）。动物的种类共有4种：猫、鱼、蛇、鸟，因此输出的可能结论有4个：
```
the animal is bird
the animal is cat
the animal is fish
the animal is snake
```

根据上面的事实和规则，我们推断得到的结论应该是`the animal is cat`。

在本专家系统中，我们使用基于**后向链接**的推理引擎。后向链接的主要思想是：从某个目标结论出发，逆向寻找能支持这个结论的规则和事实，如果能找到支持结论的相应规则和事实，则该结论成立；如果不能找到相应的规则和事实，则该结论不成立。

使用**后向链接推理**判断目标结论是否成立的算法如下：

1. 预设一个目标结论target。
2. 检查事实库，看事实库中是否存在结论target。若存在，则结论target成立，结束推理；若不存在，则进入第3步。
3. 检查规则库中的每一个规则，看结论target能否由某一个规则推断出。若存在一个规则能推断出结论target，则结论成立，结束推理；若任何规则都不能推断出结论target，则结论不成立，结束推理。

在这个算法中，最关键的是步骤3中的“看结论target能否由某一个规则推断出”，也就是说，我们需要判断结论target能否从某个规则rule中推断出来。判断方法如下：

1. 检查结论target是否是规则rule的后项。若不是，则结论target不能由规则rule推断出来，结束判断；若是，则进入步骤2。
2. 把规则rule的每一个前项作为新目标结论，递归地判断这些新目标结论是否成立（即是否能从事实库和规则库中推理出来）。若每一个新目标结论都成立，则结论target能从规则rule中推断出来，结束判断；若至少有一个新目标结论不成立，则结论结论target不能由规则rule推断出来，结束判断。

基于事实库和规则库，使用后向链接推理，我们就能推断出4个可能输出结论中哪个是成立的，即推断出该动物的种类。


## 程序说明

程序主要由三个部分组成，事实库`facts.txt`,规则库`rules.txt`，推理引擎`engine.py`。

本程序实现了规则和推理的分离，因此我们可以往规则库中添加更多的规则以扩展专家系统的功能。同时，推理引擎是与规则和事实无关的，因此我们也可以将推理引擎用于其他领域，实现不同类别、不同用途的专家系统。

程序的使用方法如下：

- 将规则按规定格式保存在规则库`rules.txt`中，示例格式如下：
```
{
IF: ['live in water']
THEN: 'the animal is fish'
DESCRIPTION: 'fish'
}
```
- 将事实按规定格式保存在事实库`facts.txt`中，每一行表示一个事实。
- 设置好要判断的目标结论，运行`infer.py`执行推理，得到的结果示例如下：
```
the animal is bird -> False
the animal is cat -> True
the animal is fish -> False
the animal is snake -> False
```

即推理程序能够判断每一条目标结论是否成立，True表示该结论成立，False表示不成立。在上面的结果中，我们可以看到，最后成立的结论是`the animal is cat`，与我们之前的预期相符，这说明了程序是正确的。


## 参考文献

- [基于规则的专家系统 -- 图形检测](https://github.com/Sorosliu1029/Rule-based_Expert_System)
