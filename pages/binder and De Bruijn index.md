- $\lambda$ 演算有两个基本的规则
  collapsed:: true
	- α-conversion:改变bound的元素，以$M[N/x]$为例，我们对M的类型讨论
	  ![image.png](../assets/image_1674612234459_0.png)
		- 如果M是一个变量/term
			- 如果M等于x，那么直接替换为N
			- 否则返回它本身
		- 如果M是一个函数Apply过程，那么分别对函数进行替换
		- 如果M是一个函数，（curry化后的）
			- 如果M bound的元素和x相同，那么函数不变（本质对应的就是变量rename）
			- 如果M bound的元素和x不同，且x中不包含自由变量（free variable）y，那么替换函数body中的x
				- 注意这里M中不包含自由变量y很重要，比如下述替换
				  $$\lambda y. 2N [N/(y)]$$
				- 下述替换中，y本来不是被y bound的，但是在替换后，由于变量名相同，所以造成替换的y被新的函数bound的错误
	- $\beta$ reduction：apply 函数的规则
- 如何实现
  collapsed:: true
	- 考虑下述替换a[v/x]的实现，如果v所有变量都是closed，也就是说没有自由变元x
	  ![image.png](../assets/image_1674613014317_0.png)
	- 当存在自由变元的情况时
	   ![image.png](../assets/image_1674617239278_0.png)
		- 我们可以将替换体里的所有同名变量都换一个新的
		- 即将上述b里面的y都用y' 表示
- De Bruijn index
	- 上述用名字绑定的方式会造成严重的歧义，所以我们可以用数字的方式表示，即德布朗指数（类似之前的nameless expr)。下面为了避免和数字冲突，我们加上$表示参数
	  $$\lambda x.x => \lambda . \$1 $$
		-
		-