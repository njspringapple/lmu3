
#### 1. 常用操作

- **生成 n 个整数数组**：

```r
# 生成 n 个数值为 value 的数组
samples <- rep(0,10)
# 快速生成 n 个数值为 0 的数组，注意，这个方法只能为 0
samples <- numeric(10)
samples <- integer(10)
```

- **for循环：**

```r
for(i in 1:10) {
	.....
}
```

- **函数定义**：

```r
my_func <- function(x,y,z) {
	...
	return(...)
}
```

#### 2. 随机变量 和 独立同分布

- **数学定义**：$X$ ~ $N(0,1)$
	- **rnorm** ：是依照正态分布的抽取概率抽取样本值的函数
	- **rnorm**(1, mean = 0, sd = 1) 或者 rnorm(**1**) 理解为 **一次样本抽取**，依照 $N(0,1)$ 的概率分布情况（例如中心点概率高，抽到的概率就高），返回 x 坐标的值
	- 因为 $X$ 是随机变量，随机变量本身就是从 $\Omega$ 样本空间映射到 $\mathbb{R}$ 的 **函数/映射**，所以可以简单理解为 **rnorm** 就是一个随机变量 $X$ 的**一次执行**，用 r 语言**伪代码**描述如下：

```r
# X 是抽取动作的 “定义”
X <- function() {
	# 从样本空间依照某个概率分布抽取一个数
	return value
}

# rnorm 是抽取动作的 “执行”，即调用rnorm(1)相当于随机变量函数 X() 调用了一次
rnorm <- function() {
	X()
}

# rnorm带了执行次数的参数后，相当于调用随机变量函数 X() 若干次
# 这返回的若干个 “数” 就是独立同分布的
rnorm <- function(repeats)  {
	# 定义返回变量，根据repeats来初始化一个空间
	result <- numeric(repeats)
	for(i in 1:repeats) {
		result[i] <- X() 
	}
	return(result)
}
```

#### 3. 指示函数

```r
# 方案1：通用指示函数
I <- function(condition) {
	# 根据输入的布尔表达式，强行转换为0或者1，因为 True 就等于 1， False 就等于 0
	as.numeric(conditon)
}

x <- c(1,2,3,4,5)
I(x > 3)
# [1]0 0 0 1 1

# 方案2：内嵌方式
x <- c(1,2,3,4,5)
indicator <- ifelse(x == 3,1,0)
indicator <- ifelse(x > 3,1,0)
```

#### 4. 求和

```r
# 基本求和：Σ xᵢ (i=1 to n)
x <- c(1, 2, 3, 4, 5)
sum(x)
# [1] 15

# 加权求和：Σ wᵢ·xᵢ
weights <- c(0.1, 0.2, 0.3, 0.2, 0.2)
sum(weights * x)

# 带函数的求和：Σ f(xᵢ)
sum(x^2)           # Σ xᵢ²
sum(sqrt(x))       # Σ √xᵢ
sum(log(x))        # Σ ln(xᵢ)

# 条件求和：Σ xᵢ·I{xᵢ > 3}
sum(x * I(x > 3))  # = 4 + 5 = 9

# 双重求和：Σ Σ f(i,j)
i <- 1:3
j <- 1:4
# 这里 function(a,b)  a* b  实际使用时候替换
my_func <- function(x,y) {
	return(...)
}
outer_sum <- sum(outer(i, j, my_func)
```

#### 5. 积分

```r
# 方法1：使用 integrate() 函数（数值积分）

# 基本积分：∫ f(x) dx from a to b
my_func <- function(x) {
	return(...)
}
result <- integrate(f, lower = 0, upper = 1)
result$value
# [1] 0.3333333  （即 1/3）

# 常见积分示例，简单函数积分采用 “内嵌” 的 “匿名” 函数，避免还要单独写个函数，起名字

# 1) ∫₀¹ x² dx
integrate(function(x) x^2, 0, 1)$value

# 2) ∫₀^∞ e^(-x) dx
integrate(function(x) exp(-x), 0, Inf)$value
# [1] 1

# 3) ∫₋∞^∞ (1/√(2π)) e^(-x²/2) dx  (标准正态)
integrate(function(x) dnorm(x), -Inf, Inf)$value
# [1] 1

# 4) 带参数的积分：∫ f(x,θ) dx
integrate(function(x, theta) x^theta, 
          lower = 0, upper = 1, theta = 2)$value

# 5) 二重积分：∫∫ f(x,y) dx dy
library(cubature)  # 需要安装
adaptIntegrate(function(p) p[1]^2 + p[2]^2, 
               lowerLimit = c(0, 0), 
               upperLimit = c(1, 1))$integral
```

#### 6. 乘积

```r
# 基本乘积：∏ xᵢ
x <- c(1, 2, 3, 4, 5)
prod(x)
# [1] 120  (即 5!)

# 阶乘：n! = ∏ i (i=1 to n)
prod(1:5)
factorial(5)
# 都是 120

# 带函数的乘积：∏ f(xᵢ)
prod(x + 1)        # ∏(xᵢ + 1)
prod(exp(x))       # ∏ e^xᵢ

# 条件乘积：∏ xᵢ·I{xᵢ > 2}
prod(x[x > 2])     # = 3·4·5 = 60

# 似然函数（常见）：L(θ) = ∏ f(xᵢ|θ)
likelihood <- function(theta, data) {
    prod(dnorm(data, mean = theta, sd = 1))
}

# 对数似然（避免下溢）
log_likelihood <- function(theta, data) {
    sum(dnorm(data, mean = theta, sd = 1, log = TRUE))
}
```

#### 7. 期望

```r
# 样本均值（经验期望）
x <- rnorm(1000)
mean(x)              # E[X] ≈ sample mean

# 离散分布的期望：E[X] = Σ xᵢ·P(X=xᵢ)
x_values <- 1:6      # 骰子
probs <- rep(1/6, 6) # 均匀概率
expectation <- sum(x_values * probs)
# [1] 3.5

# 函数的期望：E[g(X)]
g <- function(x) x^2
mean(g(x))           # E[X²]

# 条件期望：E[X | X > 0]
mean(x[x > 0])       # 只取 x > 0 的部分

# 理论期望（通过积分）
# E[X] for X~N(μ,σ²) 是 μ
result <- integrate(function(x) x * dnorm(x, mean = 2, sd = 1), 
          -Inf, Inf)
result$value
# [1] 2
```

#### 8. 方差

- **数学定义**：$Var(X) = E[(X - E[X])^2] = E[X^2] - (E[X])^2$

```r
# 样本方差
x <- rnorm(1000)
var(x)               # 样本方差, 直接使用 var 函数

# 手动计算方差
mean((x - mean(x))^2)              # Var(X) = E[(X-μ)²]
mean(x^2) - mean(x)^2              # Var(X) = E[X²] - (E[X])²

# 标准差
sd(x)
sqrt(var(x))

# 函数的方差：Var(g(X))
g <- function(x) x^2
g_x <- g(x)
var(g_x)

# 理论方差（N(0,1)的方差是1）
# Var(X) = ∫ (x-μ)² f(x) dx
integrate(function(x) (x - 0)^2 * dnorm(x), -Inf, Inf)$value
# [1] 1
```

#### 9. 概率

```r
# 经验概率（频率）
x <- rnorm(10000)
prob_positive <- mean(x > 0)     # P(X > 0)
# [1] 0.5

prob_in_range <- mean(x > -1 & x < 1)  # P(-1 < X < 1)

# 使用指示函数
mean(I(x > 0))                   # P(X > 0)

# 离散概率：P(X ∈ A)
data <- sample(1:6, 1000, replace = TRUE)
mean(data %in% c(1, 2))          # P(X ∈ {1,2})
# [1] 0.333...

# 理论概率（连续分布）
# P(X > 0) for X~N(0,1)
integrate(function(x) dnorm(x), 0, Inf)$value
# [1] 0.5

# 或使用累积分布函数
1 - pnorm(0)                     # P(X > 0) = 1 - F(0)
# [1] 0.5

pnorm(1) - pnorm(-1)             # P(-1 < X < 1)
# [1] 0.6826895
```

#### 10. 组合数学

```r
# 组合数：C(n,k)
choose(5, 2)                     # C(5,2) = 10
# [1] 10

# 排列数：P(n,k) = n!/(n-k)!
perm <- function(n, k) {
    factorial(n) / factorial(n - k)
}
perm(5, 2)                       # P(5,2) = 20
# [1] 20

# 阶乘
factorial(5)                     # 5! = 120
```