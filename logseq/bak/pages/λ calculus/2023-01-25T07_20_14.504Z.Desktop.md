- 从引入函数开始（tiny language 3）
  collapsed:: true
	- 我们继[tiny language](((639b2805-145f-444e-805a-b0fe66998f11)))需要引入新的expr定义
	  collapsed:: true
	   ![image.png](../assets/image_1673316918464_0.png){:height 113, :width 375}
		- 注意这里的Fn可以捕获环境变量，比如 x = 1 in func y -> x+y
	- 这里我们可以定义函数的解释过程
	  collapsed:: true
	  ![image.png](../assets/image_1673317570589_0.png)
		- 首先引入两个新的iterm：vadd，vmul。表示数的操作，引入这两个操作的原因是add的参数可以是函数调用
		- 然后对于函数的解释，我们会返回一个对象Vclosure，包含三个部分：环境env，参数定义xs，函数体 e
		- 调用函数的过程是：
		  collapsed:: true
			- 提取函数定义中的env，xs，e
			- 将parameter（形参） 和 argument（实参） 绑定，即作为一个新的参数环境：<parameter name, argument value>
			- 然后将捕获的环境和参数环境合并为一个env
			- 根据当前env执行函数体
	- 我们可以将上述语言转为Nameless Expr，上述的解释过程也不需要对应的参数
	  collapsed:: true
		- 转为Nameless Expr的本质是将所有的变量名都绑定到实际的栈地址上，所以解释过程和上述过程的差距是不需要任何有名参数
		- 解释过程
		  ![image.png](../assets/image_1673318085885_0.png)
			- 在这里，Fn 和 App都不需要参数
			- 在函数体中和所有的变量们都是当前执行环境env的索引，指向对应的环境变量
		- 如何计算对应的索引，homework
	-
- lambda calculus
	- Simplify tiny language 3 to lambda calculus
	  collapsed:: true
		- 对于let iterm，我们可以将其转为函数调用
		  $$let(x, e1, e2) = App(Fn(x, e2), e1)$$
		- 将所有的常量数转为函数（Church numeral）
		- 将所有的多元的参数转为单元参数（Currying，柯里化）
			- $$Fn(x_1, x_2, e) = Fn(x_1, Fn(x_2, e))$$
			- $$App(e, e1, e2) = App(App(e, e1), e2)$$
		- 这样我们就可以得到最简tiny language 3，类似lambda 演算
		  ![image.png](../assets/image_1673318570869_0.png){:height 165, :width 467}
			- 变量名
			- 一元函数定义
			- 一元函数调用
	- Formally Definition
	  collapsed:: true
		- Lambda terms: $M,N ::= x|(MN)|(\lambda x.M)$
			- 其中小写字母x，作为变量名
			- MN: 是函数调用，即将N作为M的参数
			- $\lambda x.M$ 是函数定义，其中参数是x，函数体是M
		- computation
			- 计算过程也就是函数调用，称为 $\beta - reduction$
			- $\beta - redex$ 是一个term，形如 $(\lambda x. M)N$，表示$\beta - reduction$操作的对象
			- $\beta - reduction$ 是将 一个 $\beta - redex$ 归约到 $M[N/x]$，即将M里所有的x替换为M
			- 例子：
			  ![image.png](../assets/image_1673319023444_0.png)
		- Confluence of untyped lambda calculus
			- lambda 计算的结果和计算的顺序无关，也就是说我们可以随便选择一个 $\beta - redex$进行归约
			- 形式化的描述
			  ![image.png](../assets/image_1673319704511_0.png)
	- Execution
	  collapsed:: true
		- 形式化执行
			- 如何执行函数Call by Value
			  $$\frac{}{(\lambda x.a)v \rightarrow a[v/x]} $$
			- 执行顺序，从左到右
			  $$\frac{a\rightarrow a'}{ab\rightarrow a'b}, \frac{b\rightarrow b'}{vb\rightarrow vb'}$$
		- 具体的解释执行可分为两种：Substitution 和 Environment
			- 在执行的时候，替换对应变量，正如上述定义
			  ![image.png](../assets/image_1673359740048_0.png){:height 344, :width 392}
				- 实际不容易实现，因为生成代码很麻烦
				- 在编译时可以做一些IR的生成，类似于常量折叠，partial evaluation？
			- 在执行的时候，将所有的外部变量，都保存在环境中，最后统一解释执行替换
			  ![image.png](../assets/image_1673359862981_0.png){:height 255, :width 345}
	- 用函数表示数
	  collapsed:: true
		- bool
		  collapsed:: true
			- 数据定义：$T = \lambda xy.x , F = \lambda xy.y$，即它接收两个参数，即if else里的两个参数
			- 基本操作if-then-else $\lambda x.x$
				- 当我们带入T，即$\lambda x.x T MN= \lambda xy.x MN = M$。可见正好选择了第一个参数，和true语意相通
				- 同理，当我们带入F的时候，会选择N
		- Pair
		  collapsed:: true
			- 数据定义：$P = \lambda xyz.zxy$，它接受三个参数，pair 对xy和访问方法p
			- First：F=$\lambda p.p \lambda xy.x$
				- 规约过程
				  id:: 63bd7690-255f-4331-9138-7c191cfc0a19
				  $$
				  F P M N \rightarrow F \space \lambda z.zMN  \rightarrow \lambda z.zMN \space \lambda xy.x \rightarrow \lambda xy.x MN  \rightarrow M
				  $$
			- Second: $F = \lambda p.p \lambda xy.y$
		- Numerals
			- 数据定义
			  $$
			  n = \lambda fx.f^n(x) \\0 = \lambda fx.x $$
			- 数据操作
			  collapsed:: true
				- 加法推导
					- 后继函数: succ
					  $$succ = \lambda nfx.f(nfx)$$
						- 举例
						  $$
						  \begin{align}
						  succ(n)  &\\
						  &= \lambda nfx.f(nfx) \lambda fx.f^n(x)  \\  
						  &= \lambda fx.f(\lambda fx.f^n(x)fx) \\
						  &= \lambda fx.f(f^n(x)) = \lambda fx.f^{n+1}(x)
						  \end{align}
						   $$
					- 加函数：add
					  collapsed:: true
					  $$\lambda nmfx.nf(mfx)$$
						- 推导
						  $$
						  \begin{align}
						  \lambda nmfx.nf(mfx) \space nm \\
						  &= \lambda fx.(\lambda fx.f^n(x)) f (\lambda fx.f^m(x)fx) \\
						  &= \lambda fx.(\lambda fx.f^n(x)) f^{m+1}(x)) \text{每次带入两个f} \\
						  &= \lambda fx.( f^{m+n}(x)) 
						  \end{align}
						  $$
					- is zero
					  collapsed:: true
					  $$iszero = \lambda n.n(\lambda z.\overline F)\overline T$$
						- 推导
							- 当是0的时候
							  $$\lambda n.n(\lambda z.\overline F)\overline T 0 = \lambda fx.x(\lambda z.\overline F)\overline T = \overline T$$
							- 当不是0
							  $$\lambda n.n(\lambda z.\overline F)\overline T 0 = \lambda fx.f^nx(\lambda z.\overline F)\overline T = \overline F$$
					- Predecessor
					  collapsed:: true
					  $$\lambda n. fst (n \space(\lambda p. pair(snd\space p)(succ (snd\space p)))(pair 0, 0)$$
						- 推导
							- 0
							  $$
							  \begin{align}
							  & \lambda n. fst (n \space(\lambda p. pair(snd\space p)(succ (snd\space p)))(pair 0, 0) 0 \\
							  &= fst (0 \space(\lambda p. pair(snd\space p)(succ (snd\space p))(pair 0, 0)  \\
							  &= fst(pair 0, 0)  = 0
							  \end{align}
							  $$
							- 非0
							  $$
							  \begin{align}
							  & \lambda n. fst (n \space(\lambda p. pair(snd\space p)(succ (snd\space p)))(pair 0, 0) n \\
							  &= fst (n \space(\lambda p. pair(snd\space p)(succ (snd\space p))(pair 0, 0)  \\
							  &= fst (\space(\lambda p. pair(snd\space p)(succ (snd\space p))^n(pair 0, 0) \\
							  &= fst (\space(\lambda p. pair(snd\space p)(succ (snd\space p))^{n-1}(pair 0, 1) \\
							  &= fst(n-1, n) = n-1 
							  \end{align}
							  $$
						-
					- multiplication
					  collapsed:: true
						- 递归定义
						  $$\times = \lambda nm. ifZero \space n \space 0 (\space m+(n-1)\times m)$$
						- 上述公式可以将其化简，提取乘号
						  $$\times = \lambda nm. ifZero \space n \space 0 \space (\lambda f. (m+f(n-1, m)\times))$$
						- 将右边的公式用F表示即得到
						  $$\times = F \times$$
						- 接下来的核心便是对F的推导，这部分的核心是lambda对于递归的定义，感觉和乘法无关。参照下一节
						-
		- 递归：如何将递归引入lambda 演算
		  collapsed:: true
			- 这里以一个经典斐波拉契数列为例
				- 斐波拉契函数
				  $$Fib = \lambda n. if (n = 1) \text{ 1 } (n * f(n-1))$$
				- 上述函数有一个问题，就是f是递归定义的，而lambda 语义中没有递归定义。我们可以抽象为
				  $$Fib = \lambda fn.if(n=1)1(nf(n-1))Fib$$
				- 我们将左半边抽象为F得到
				  $$Fib = \lambda fn.FFib$$
				- 由于n是未bound的，所以我们得到
				  $$f = Ff\\F= \lambda fn.if(n=1)1(nf(n-1))$$
				- 这里我们希望求解f，而f的本质上的语义是递归的调用F（推导略）
					- 不动点理论当存在一个形式$YF = F(YF)$，我们把Y称为不动点组合子
					- 经典Y组合子(Y combinator)
					  $$\lambda f. (\lambda x.f(xx))(\lambda x.f(xx))$$
				- 所以我们可以解得$Fib= YF$，即
				  $$Fib = Y\lambda fn.if(n=1)1(nf(n-1))$$
					- 推到$Fib(2)$
					  $$\begin{align}
					  Fib2 &= YF2\\
					  &= \lambda x.F(xx) \lambda x.F(xx) 2 \\
					  &= F(YF)2 \\
					  &=(\lambda f.2f(1) )(YF) \\
					  &=2*F((YF))1 \\
					  &=2*1 \\
					  &=2\end{align}$$
				- 程序语言来实现
					- 假设存在一个递归函数fix
					- 我们可以定义一个通用的调用改递归函数的模版:
					  ```
					  combinator (fix, ...) ->  fix (combinator, ...)
					  ```
					- 这里还是以递归函数为例
					  ```
					  fib(n) = n == 1? 1 : fib(n-1)
					  combinator(fix, n) ->  fix(combinator, n)  
					  ```
					- 该定义的好处，我们可以在combinator函数中做一些通用的优化，比如cache缓存结果等