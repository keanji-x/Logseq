title:: Plan Space Analysis: An Early Warning System to Detect Plan Regressions in Cost-based Optimizers

- 问题
	- 如何衡量优化器修改的衡量，Plan regression
		- 一个修改可以有利于一些计划而不利于另一些计划
	- 传统工作只考虑BPF：即会不会影响最优的计划
	- 本文考虑了整个搜索空间
- 标准范式
	- 标准流程（mtr）
		- 定义测试的query 集合，并保存它们的优化计划
		- 当修改优化器时，
	-
	-