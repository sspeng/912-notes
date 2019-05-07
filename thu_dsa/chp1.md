数据结构与算法第一章知识总结
=========================

## 为什么需要数据结构和算法

讨论这个问题之前，先想想究竟什么是计算机，又什么是算法。

_Computer science should be called computing science, for the same reason why surgery is not called knife science.
-- E. Dijkstra_

## 几个实例

+ 绳索计算机以及计算垂直线的算法
+ 尺规计算机以及计算三等分点的算法。
+ 现代电子计算机以及冒泡排序的算法。

## 什么是算法

上面几个例子其实并没有本质的区别，几千年前的人们就开始研究了各种各样的算法，现代人还在重复这个研究工作。

从上面的例子中，我们可以总结出算法的基本概念：

+ 所谓计算，本质上是对信息处理的过程。对输入的数据按照机械的步骤，执行得出结果的过程。
+ 所谓计算模型，如上面的绳索和尺规，就是计算机，即信息处理的工具。
+ 这样的话，算法就是在特定的计算模型下，旨在解决问题的指令序列
	- 输入
	- 输出
	- 正确性
	- 确定性
	- 可行性
	- 有穷性: 如Hailstone问题，尚未证实其有穷性
	- ...

评价一个算法，常常具有上面这许多因素。在算法正确的基础上，我们会比较关注一个算法的代价，即它运行的时间与空间。

## 算法的评价

_Algorithm + Data Structure = Programs // N.Wirth, 1976_

_(Algorithm + Data Structure) x Efficiency = Computation // Pro Deng_

对于求解同一个问题，往往会存在许多不同的算法。问题在于，应该如何评价这些算法的优劣呢？

### 度量的基准

_To measure is to know. If you cannot measure it, you cannot improve it
-- Lord Kelvin_

考虑我们要评价两个不同算法的优劣，一个很自然的思想是实验测量，通过测量这两个算法在同一问题实例(instance)P的情况下，两个算法的运行代价。即

$$T_A(P) = the\ cost\ of\ algorithm\ A\ to\ solve\ instance\ P$$

可是，这样的测量真的有意义吗？

很明显，上面的方法存在下面的问题。针对某个问题实例的测量，并不能反映算法在实际运行时的性能。因为实际运行时的问题实例有很多，不同实例之间算法的性能很难说有相关性。

那好，我们对不同的问题实例进行抽象。根据经验，一般的算法的运行代价和问题的规模是相关的。一般说来，问题的规模越大，算法运行的代价越大(当然也有反例，如Hailstone)。这样，我们就可以把基于问题实例P的测量，转化为基于一系列规模为n的问题实例P的测量，即

$$ T_A(n) = the\ cost\ of\ algorithm\ to\ solve\ instances\ of\ scale\ n $$

有下面两种方案：

+ 最坏情况下的运行代价
+ 平均情况下的运行代价

实际上，多数情况下，我们都是关注最坏情况下算法的运行代价。

### 理想模型

有了上面的讨论后，终于可以评价算法的优劣的吧？其实还不行。因为实验测量的方法还会受到外界因素的影响。

例如绳索计算机以及尺规计算机，其运行效率与操作人员的熟练程度，当天状态是息息相关的。现代计算机也是一样，即使使用同一台机器运行两个算法，也会由于进程的调度，硬盘的读写情况不同而多少有所差异。所以，实验测量的方式是可以的，但是是不准确的。

由上面的讨论，我们需要的是理想的平台或是模型，使我们可以不再依赖于上面的各种不确定因素，从而直接准确地描述并测量算法。

+ 图灵机TM(Turning Machine)
	- 有一条纸带(tape), 上面均匀地划分了小格，格子里面是字符，默认为'#'
	- 字符(alphabet)的个数是有限的
	- 存在一个头指针(Head),指向纸带上的某一个单元格，可以读写其中的字符
	- 头指针存在状态(State), 也是TM的状态。每一步都可以改变自己的状态
	- 每一步会执行一个传递函数`Transition_Function(q, c; d, L/R, p)`: 当前状态为q且当前字符为c，将当前字符改写成d， 当前状态改写成p，并且向左L或向右R移动。一旦转入特定状态'h'(for halt)，则进入停机。
	- 实例：将二进制非负整数加一

+ Random Access Machine
	- 具有一些顺序编号的寄存器R[0], R[1], R[2]...总数没有限制
	- 具有一些基本操作，都只需要花费常数时间

	```
	R[i] <- c     		R[i] <- R[R[j]]			R[i] <- R[j] + R[k]
	R[i] <- R[j]		R[R[i]] <- R[j]			R[i] <- R[j] - R[k]
	IF R[i] = 0 GOTO l 	IF R[i] > 0 GOTO l 		GOTO l 		STOP
	```

	- 实例：floor向下取整的除法(就跟汇编似的)

可以看到，TM模型和RAM模型一样，都是把运算抽象成了各个基本操作的叠加。而这些基本操作的运行时间是固定的，平台无关的。这样我们就可以独立于具体的平台，对算法的效率做出可信的比较与评价。

由于每步基本操作的运行时间是固定的，因此我们可以使用算法执行所需要的基本操作的次数来评价一个算法。这样

$$T(n) = number\ of\ basic\ operations\ to\ solve\ a\ problem\ of\ scale\ n$$

### big O notation

前面说到通过建立的理想模型，我们可以把算法运行的时间，转化成在理想模型上运行的基本操作的次数。从而我们已经可以对不同的算法进行比较。这样的比较绝对是严谨的，只是在操作上还存在一些问题。

容易设想到，不同的算法针对不同规模输入的数据，其表现也不会相同。一些算法在小规模输入时表现良好，另一些则恰恰相反，在大规模输入时表现突出。这对应与两个算法的T(n)并不是严格的大于或者小于关系，而是一个此消彼长的过程。这种情况对于我们比较算法的优劣是不利的，因此有必要采取一些别的措施。

在评价算法的效率时，其实对于小规模的问题，我们往往不太关心它的效率，这是因为此时不同的算法的差异并不明显。因此，我们往往更关注于更大规模问题的效率。这种着眼长远，更注重时间复杂度的总体变化趋势的分析方法，即渐进分析(asymptotic analysis)。

存在不同的渐进分析方法。回忆我们之间提到，我们往往是评价一个算法在最坏情况下的效率，这里引入大O记号(big O notation),其定义如下：

_若存在函数f(n)和正的常数c，使得在渐进的条件(n >> 2)下，T(n) <= c·f(n)，则可以认为f(n)给出了T(n)增长速度的一个上界(最坏情况)，记T(n) = O(f(n))_

同时，根据定义，可以给出大O记号的几个性质:
+ 对于任意常数 c > 0, O(f(n)) = O(c·f(n))
+ 对于任意常数 a > b > 0, O(n^a + n^b) = O(n^a)

要注意的是，性质2表面上是忽略了多项式中的低次项，本质上，是在大O记号的基础上进行了放大，然后忽略掉常数项

$$n^a + n^b <= n^a + n^a = 2 * n^a = O(n^a)$$

在计算大O记号时，可以直接忽略低次项的原因也在于此。

### Other notations

上面也说了，大O记号表示的是最坏情况下算法在时间复杂度上的增长趋势。除此以外，还存在在最好情况下，以及平均情况下的时间估计。

#### big $\Omega$ notation

big $\Omega$ notation是标志了算法运行时间的下界，即最好情况，其定义如下:

_若存在函数f(n)和正的常数c，使得在渐进的条件(n >> 2)下，T(n) >= c·f(n)，则可以认为f(n)给出了T(n)增长速度的一个下界(最好情况)，记T(n) = $\Omega$(f(n))_

#### big $\Theta$ notation

big $\Theta$ notation 是标志算法运行时间的平均情况，它也具有相似的定义:

_若存在函数f(n)和正的常数c1和c2，使得在渐进的条件(n >> 2)下，c1·f(n) < =T(n) <= c2·f(n)，则可以认为f(n)给出了T(n)增长速度的平均情况，记T(n) = $\Theta$(f(n))_


## 大O记号的运用：计算

接下来是讨论大O记法应该怎么用，具体说来就是如何计算大O记法下算法的时间复杂度。这又分为两种情况，即迭代的计算和递归的计算。这是因为，其他非迭代以及非递归的算法都是常数的时间复杂度，在渐进的条件下可以忽略。

### 循环(迭代)算法大O的计算：级数求和

其实，简单说来，就是分析各层循环执行的次数，然后求和。在渐进的条件下，就是级数求和问题。

所以要进行计算循环的时间复杂度，首先需要知道各种常见级数的求和方法，现列举如下:

+ 幂方级数: 比幂次高出一阶
	- $T_2(n) = 1^2 + 2^2 + 3^2 + ... + n^2 = \frac{n(n+1)(2n+1)}{6} = O(n^3)$
	- $T_3(n) = 1^3 + 2^3 + 3^3 + ... + n^3 = \frac{n^{2}(n+1)^{2}}{4} = O(n^4)$
	- $T_4(n) = 1^4 + 2^4 + 3^4 + ... + n^4 = \frac{n(n+1)(2n+1)(3n^{2}+3n-1)}{30} = O(n^3)$
	- ...
	- $T_k(n) = 1^k + 2^k + 3^k + ... + n^k = \int_{0}^{n}x^k{d}x = \frac{x^{k+1}}{k+1}|_{0}^{n} = O(n^{k+1}))$
+ 几何级数: 与末项同阶
+ 收敛级数
+ 不收敛的级数
	- 调和级数
	- 对数级数 $log1 + log2 + log3 + ... + logn = log(n!) = \Theta(nlogn)$

邓公说，计算的时候还是要灵活对待。最好可以画一个图分析，图上的每一个点代表一次循环，这样图的面积就是时间复杂度。

### 递归算法的大O计算

递归算法计算时间复杂度一般就有两个方法:

+ 递归跟踪(recursion trace): 通过跟踪到每一个递归调用，其总和的运行时间，就是算法的执行时间。

+ 递推方程(recurrence): 通过递推的表达式，写出不同规模问题运行时间的递推公式，通过求解递推公式，从而求解。

详情可见邓公的教材和讲义，有一些实例。

## 动态规划

动态规划是一种程序设计思想。即分析程序运行的时间复杂度以及其机理，从而不断地优化程序的执行时间。例如重新设计算法，以避免递归过程中的重复操作。或者直接化递归程序为迭代程序。

一般设计程序时，很难一开始就想出一个完美的解，一般人都是先想出一个易于实现的，可以运行的算法。但是其时间复杂度往往很高，例如递归的程序，由于逻辑简单，一般都容易想出一个递归的版本，但是递归的程序往往开销过大。

在有了一个可以运行的版本后，再分析已有程序的运行过程，对其进行优化，比如规避递归程序的冗余，或者利用栈模拟操作系统的函数栈，写一个非递归的代码。这整个过程就是动态规划。

关于动态规划，主要是有几个例子

+ 斐波拉契数
+ LCS

可以查看邓公的网课和讲义，我自己也有代码实现。