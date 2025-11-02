
>[!note] 密度变换
>**密度变换**：已知概率密度 $f_X(x)$  和 变换函数 $Y=g(x)$，例如：$Y=X^2$，求 $f_Y(y)$
>**一维**：$f_Y(y)=f_X(g^{-1}(y)) \cdot |\frac{1}{g'(g^{-1}(y))}|$   **简易公式**：设$h(y)=g^{-1}(y),f_Y(y)=f_X(h(y))\cdot |h'(y)|$
>
> 

>[!note] 线性变换
> **线性变换**：$X$~$N(\mu,\sigma^2),Y=aX+b,Y$~$N(a\mu+b,a^2\sigma^2)$ - **可以用密度变换证明**
>  **标准化**：$X$~$N(\mu,\sigma^2) \Rightarrow Y=\frac{X-\mu}{\sigma}$~$N(0,1)$ - **根据线性变换公式**
> - $X$ ~ $N(\mu,\sigma^2) \rightarrow aX$ ~ $N(a\mu,a^2\sigma^2)$ - 乘法，分布内部两个都乘，方差前面乘以平方数
> - $X$ ~ $N(\mu,\sigma^2) \rightarrow X+b$ ~ $N(\mu+b, \sigma^2)$ - 加法，只有期望前面加

>[!note] 正态分布标准化
> 
>  **标准化**：$X$~$N(\mu,\sigma^2) \Rightarrow Y=\frac{X-\mu}{\sigma}$~$N(0,1)$ - **根据线性变换公式**
> 

>[!note] 中心极限定理（CLT）
> 
>  大量 “**独立同分布**” 随机变量的 “**和**”，经过 ”**标准化**“ 后，其分布会 “**收敛**” 到钟形的正态分布，期望=0，方差=1
> - $X_1,...,X_n$ 是独立同分布的随机变量序列
> - $S_n = X_1 + ... + X_n$
> - 当 n 趋近于无穷时，$S_n \xrightarrow{D} N(n\mu,n\sigma^2)$
> - 因此，均值 $\bar{X_n} \xrightarrow{D} N(\mu,\frac{\sigma^2}{n})$
> - 进行正态分布标准化，得到：$\frac{\bar{X_n}-\mu}{\frac{\sigma}{\sqrt{n}}} \xrightarrow{D} N(0,1)$


- **单次伯努利实验的事件**：**rbinom**(1,1,0.75)
- **多次伯努利实验的事件集合**：**rbinom**(100,1,0.75)
	- hist(**rbinom**(100,1,0.75),ylim = c(0,10000))
- **二项随机变量**：**rbinom**(1,100,0.75) 
	- 二项随机变量是 **重复 n 次伯努利实验的和**，$Y = X_1 + ... + X_n$ ~ $B(n,p)$
	- 等价于：sum(**rbinom**(100,1,0.75))
- **二项分布**：多个二项随机变量的样本，$Y_1,...,Y_m$ 可以用来观测二项分布
	- hist(replicate(1000,sum(**rbinom**(100,1,0.75))),ylim = c(0,0.15), probability = TRUE)
	- hist(replicate(1000,mean(**rbinom**(100,1,0.75))),ylim = c(0,15),probability = TRUE)
	- hist(replicate(1000,100 * mean(**rbinom**(100,1,0.75))),ylim = c(0,0.2),probability = TRUE)
- **指数分布**：X ~ $E(\lambda)$ 
	- **回顾**：指数分布常用于描述 “**两次随机事件发生的间隔时间**”，短时间内发生事件的概率越大，**说明事件间隔时间越短，短时间内发生事件的概率越大**。
	- $E(X) = \frac{1}{\lambda}$，$Var(X)=\frac{1}{\lambda^2}$, $\lambda > 0, T = \mathbb{R}_+^0$
	- **直方图**：hist(**rexp**(100,1)) 
	- **x轴** - 两次随机事件发生的间隔
- **泊松分布**：X ~ $\mathscr{P}(\lambda)$
	- **回顾**：描述单位时间 / 空间内 “**稀有事件**” 发生的次数 -- 事件发生的频率，例如 $\lambda = 1$，在客服电话事件中意味着平均每个小时接听 1 次电话，但是有可能有的小时接听 2 次，有的小时没有电话
	- $E(X) = \lambda, Var(X) = \lambda$
	- **直方图**：hist(**rpois**(100,1),breaks = seq(-0.5, 10.5, 1))
	- **x轴** - 单位时间发生时间次数

