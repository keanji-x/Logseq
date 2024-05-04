- Beam model
  collapsed:: true
	- Terminology：What is streaming
		- 严格定义 ![image.jpg](../assets/29881c23-6aae-4406-8204-e9d2ab6830c5-1115003.jpg)
			- 与之相对的是，Table 和 Stream对应bounded data和 unbounded data
	- Capabilities：Limitation in streaming
	  collapsed:: true
		- 优点：延迟低
		- 缺点：推断的结果不够准确（因为利用的数据有限）
		- lamvda 架构：两个执行器 ![image.jpg](../assets/22cd7e42-b668-4418-95ea-21eb10b4d707-1115003.jpg)
			- 一个提供流计算，负责低延迟
			- 一个提供批计算，负责处理那些数据，保证高准确
		- Kappa：流是批的超集
-