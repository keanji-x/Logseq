- 背景
	- 有Index的Nest Loop Join 要比hash join的扩展性更强
		- Index 的读取是 O(log n)
		  collapsed:: true
			- 这里忽略了平时对Index的维护需要更多的写
			- 但是对于AP，是否这是有价值的
		- 而 hash join 和 sorting 的 操作是O(n)