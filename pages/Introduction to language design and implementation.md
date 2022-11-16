- 从设计一个语言开始，表达式evaluation 语言
	- Tiny Language 0：我们使用Add来构建抽象语法树，避免具体的语义
	  ![image.png](../assets/image_1668603424226_0.png){:height 147, :width 499}
		- 如果我们想要解释它的话，可以使用模式匹配来递归执行
		  collapsed:: true
		  ![image.png](../assets/image_1668603512443_0.png){:height 135, :width 383}
			- 上述的执行语义可以转为数学符号描述，formalization
				- 定义基本的term：符号，和值范围values
				  ![image.png](../assets/image_1668603567971_0.png){:height 68, :width 523}
				- 对于上述的三条执行规则，我们可以用数学语言描述
				  ![image.png](../assets/image_1668603598358_0.png)
			- 上述的执行过程依赖于一个语言栈，即我们需要保存临时变量
		- Lowering to a stack machine and interpret：引入一个存储栈去执行
			- 定义基本操作
				- 指令
				  ![image.png](../assets/image_1668603924453_0.png)
				- 程序
				- ![image.png](../assets/image_1668603932922_0.png){:height 53, :width 486}
				- 操作数
				  ![image.png](../assets/image_1668603950876_0.png){:height 40, :width 352}
				- 数据栈
				  ![image.png](../assets/image_1668603960302_0.png){:height 43, :width 474}
			- 基于栈的数据操作
			  ![image.png](../assets/image_1668603999064_0.png){:height 215, :width 522}
				- Cst：将数据压入栈顶
				- Add|Mul：取出栈顶的两个元素来操作
			- formalization
				- 压栈出栈的数学符号
				  ![image.png](../assets/image_1668604078599_0.png){:height 107, :width 500}
				- 基本的操作指令和数学符号的映射
				  ![image.png](../assets/image_1668604138573_0.png)
				- formalization
					- 这里我们使用方括号表示编译，则我们得到对语言的编译规则为
					  ![image.png](../assets/image_1668604312038_0.png){:height 152, :width 492}
					- 编译对应的语义为
					  ![image.png](../assets/image_1668604358664_0.png){:height 76, :width 426}
					-
			-
			-
			-
	-
		-
		-
		-
		-