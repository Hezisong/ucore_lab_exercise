对实验报告的要求：

列出你认为本实验中重要的知识点，以及与对应的OS原理中的知识点，并简要说明你对二者的含义，关系，差异等方面的理解（也可能出现实验中的知识点没有对应的原理知识点）

	1. 操作系统的调度管理机制
	2. ucore的系统调度器框架，以及缺省的Round-Robin调度算法
	3. Stride Scheduling调度算法

列出你认为OS原理中很重要，但在实验中没有对应上的知识点

	没有分析每种调度算法的优劣，如果能够通过测例来分析每种调度算法的性能，会对这方面的知识有更深层次的认识

练习1: 使用 Round Robin 调度算法（不需要编码）

请理解并分析sched_calss中各个函数指针的用法，并接合Round Robin 调度算法描ucore的调度执行过程

	函数指针用法如下:
	init:初始化进程运行队列
	enqueue:将新进程加入队列
	dequeue:将进程移出队列
	pick_next:从队列中选择下一个运行的进程
	proc_tick:在每个时钟周期更新进程运行信息

Round Robin调度算法:

	1. 算法思路:时间片结束时，按FCFS算法切换到下一个就绪进程
	2. enqueue:将进程加入队列，设置进程时间片信息
	3. dequeue:将进程移出队列
	4. pick_next:按照FCFS算法选择下一个运行的进程
	5. proc_tick:在每个时钟周期更新进程时间片信息，并判断是否需要schedule


请在实验报告中简要说明如何设计实现”多级反馈队列调度算法“，给出概要设计，鼓励给出详细设计

	1. 将就绪队列按优先级分为多个子队列
	2. 每个队列有自己的调度策略
	3. 不同队列之间的调度采用时间片轮转方式，使每个队列都得到一个确定的能够调度其进程的CPU总时间
	4. 进程的时间片大小随优先级增加而增加；如果进程在当前时间片没有运行完毕，则降至下一个优先级
	
	详细设计：
	1. proc_init:建立就绪队列数组，下标代表优先级
	2. enqueue:每个进程进入相应优先级的队列
	3. 执行时每个进程的时间片根据其队列的优先级而定
	4. proc_tick:时间片结束时，未完成的进程的优先级下降一级
	5. pick_next:选取下一个进程时，首先根据时间片轮转选择队列，再在队列中选择进程


练习2: 实现 Stride Scheduling 调度算法（需要编码）

实现过程如下:
	1. proc_init:初始化run_list,proc_num,run_pool
	2. enqueue:将进程加入队列并初始化其时间片信息
	3. proc_tick:更新时间片信息，并判断是否需要调度
	4. pick_next:选取当前stride最小的进程作为下一个需要运行的进程