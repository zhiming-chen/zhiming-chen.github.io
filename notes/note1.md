---
layout: default
title: 笔记1
permalink: /notes/note1.html
---

#  R语言基础
---
> 这部分内容之前在其他栏目里写过，后续会逐步搬运到这个文件中来


---



#  算法基础及案例
---

> 学习掌握算法基础，并在R语言中实现。
> 因为R语言在很多算法问题上做不到像C，java 甚至Python那样灵活，所以，这部分会是一些入门级别的简单算法案例。


---
##  W1D1 质数判断--初步认识选择、判断与循环


---

##  W1D2 最大公约数--初步认识循环与递归

---

##  W1D3 Hanoi 塔问题-- 递归算法的初识
---


###  递归的本质是“自我调用”

递归算法的核心思想是：
 
> 把一个**大问题**，逐步**分解成规模更小的相同问题**，直到可以用**直接答案**解决的**最小问题**为止。


###   汉诺塔问题概述

> 给定三根柱子 A、B、C，把 N 个大小不同的圆盘从 A 移到 C，每次只能移动一个，且**大盘不能叠在小盘上**。


###   递归思想分析

对于 `n` 个盘子（编号从大到小为 n, n-1, ..., 1）：

1. **把前 n-1 个盘子** 从 A 移到 B（借助 C）→ 递归①
    
2. **把第 n 个盘子** 从 A 移到 C（一步）
    
3. **把前 n-1 个盘子** 从 B 移到 C（借助 A）→ 递归②


###   可视化递归流程（n=3）：

```
 hanoi(3, A, C, B)    # 表示把3个盘子从A搬到C，用B作为辅助
├── hanoi(2, A, B, C)
│   ├── hanoi(1, A, C, B)
│   └── Move disk 2 from A to B
│   └── hanoi(1, C, B, A)
├── Move disk 3 from A to C
└── hanoi(2, B, C, A)
    ├── hanoi(1, B, A, C)
    └── Move disk 2 from B to C
    └── hanoi(1, A, C, B)
    
```


---

###   R语言实现

```

hanoi <- function(n, from = "A", to = "C", aux = "B") {
  if (n == 1) {
    cat(sprintf("Move disk 1 from %s to %s\n", from, to))
  } else {
    hanoi(n - 1, from, aux, to)                       # 步骤①
    cat(sprintf("Move disk %d from %s to %s\n", n, from, to))  # 步骤②
    hanoi(n - 1, aux, to, from)                       # 步骤③
  }
}


```

###   运行示例

```
hanoi(3)

Move disk 1 from A to C
Move disk 2 from A to B
Move disk 1 from C to B
Move disk 3 from A to C
Move disk 1 from B to A
Move disk 2 from B to C
Move disk 1 from A to C

```




##  W1D4 Fibonacci 斐波那契数列--递归算法初始与替换

###  斐波那契数列简介

+ 数列的前两项分别为0,1
+ 从第三项开始，每项数值为其前两项数值之和，如第三项为1，第四项为1+1=2，第五项为1+2 = 3，第六项等于2+3 =5...
+ f(0) =0,f(1)=1,f(n) =f(n-1) + f(n-2) (n>=3)

### 递归思想分析

求斐波那契数列，是一个典型的递归算法问题。对于一个长度为`n`的数列{$ x_1, x_2,x_3....x_(n-1),x_n $}

### R语言实现

```

fibonacci_1  <- function(n){
    if (!is.numeric(n) || n %% 1 != 0 || n < 0) {
        stop("请输入非负整数")}
    
    if(n == 0) return(0)
    if(n == 1) return(1) 
    fibonacci_1(n-1) + fibonacci_1(n-2)
   
}

# fibonacci_1(6)

```

但是，这个算法效率太低了，大量重复计算，指数级的复杂度。比如计算f(5)时，需要计算F(4)和F(3),计算F(4)时又需要计算F(3)和F(2)...大量的重复计算。

可以使用其他方式实现，比如带缓存或循环法。

###  循环法实现

做法其实很简单，F(1)=0,F(2)=1,后面每次计算结果存储起来，作为后面数值的计算输入。

在编程里后续会有大量这样的案例，有些是直接数字替换，有些是存为向量结果。也就是这里提及的缓存的概念。

```

fibonacci_2 <- function(n){
    if (!is.numeric(n) || n %% 1 != 0 || n < 0) {
        stop("请输入非负整数")}
    
    if(n == 0) return(0)
    if(n == 1) return(1) 
    
    a <- 0
    b <- 1
    
    
    for(i in 2:n){
        n_fib <- a + b
        a <- b
        b <- n_fib
    }
    
  return(b)    
}

# fibonacci_2(6)

```

### 存储法

```

fibonacci_3 <- function(n){
    if (!is.numeric(n) || n %% 1 != 0 || n < 0) {
        stop("请输入非负整数")}
    
    if(n == 0) return(0)
    if(n == 1) return(1) 
 
    #创建一个长度为n+1的向量
 fi_v <- numeric(n+1)
 fi_v[1] <- 0  # 这里存放F(0)
 fi_v[2] <- 1  # 这里存放F(1)
 
 for (i in 3:(n+1)){# 这里n+1 需要括起来...
     fi_v[i] <- fi_v[i-1] + fi_v[i-2]
 }
 return(list(fi_v[n+1], fi_v))
 
}
# 和很多其他编程语言不一样，R语言向量第一位用数字1表示，不是其他语言多用的0    
    
# fibonacci_3(6)

```

这里的迭代法与缓存法，在后续的编程中会大量出现。


##  W1D5 返回 1 到 n 的所有质数

本节使用W1D1写的`is_prme`函数来实现质数的查找来实践：
+ 数据储存到向量
+ for循环
+ apply系列循环初识
+ map循环初识
+ 向量化编程思维初启...

### 实现步骤概述如下

+ 调用自己写的函数：`source`函数实现
+ 对数据对象循环实施is_prime函数
+ 常用的三种方法下文分别列出

#### for 循环 + 向量存储

+ 函数输入为数字n, 
+ 函数实现从1到n中全部质数的查找
+ 这些质数存储到一个向量
+ 函数的输出为这个向量
+ 首先常见一个空向量
+ 对1:n之间的数值用is_prime函数进行遍历，结果为TRUE 时存入空向量，结果为false不存，直到n为止。
举例：
+ 先创建一个空向量p <- c()
+ 对于数值2，他是质数，所以向量就变成了p  : c(p, 2), 结束，继续下一步
+ 对于数值3，他也是质数，所以向量就变成了p :c(p,2,3)...
+ 因为初始p为空向量，所以最后就实现了目标数值的存储。

```

# for 循环 + 向量存储

get_prime_for <- function(n){
    # 目标位找到1到n之间的全部质数
    
    if (!(is.numeric(n) && n > 1 && n %% 1 == 0)) {
        stop("请输入大于1的整数")
    }
    
    primes <- c()
    
    for(i in 2:n){# 因为1不是质数，所以不纳入
        
        if(is_prime_for(i)){ # 调用的函数，他的输出是TRUE|FALSE 的逻辑值
            
            primes <- c(primes,i)
        }
     
    }
    
    return(primes)   # 不能写在for循环里，否则他会直接在第一步就结束for循环
}


# get_prime_for(19)

```



#### 输入向量+apply系列函数循环 + 向量输出

+ 数字n作为输入
+ 根据数字n生成1,2,3...n构成的向量
+ 使用apply函数对这个向量遍历is_prime函数，对向量元素进行判断T|F
+ 提取全部T向量元素，形成向量，作为函数的输出

```

get_prime_apply <- function(n){
    
    
    
    # 目标位找到1到n之间的全部质数
    
    if (!(is.numeric(n) && n > 1 && n %% 1 == 0)) {
        stop("请输入大于1的整数")
    }
    
    vec <- 2:n
    
    vec[sapply(vec,is_prime_for)]
     
    }
    


# get_prime_apply(19)

```

#### 输入向量+map循环 + 向量输出

简单记录map循环

```
## map 循环

get_prime_map <- function(n){
    
    
    
    # 目标位找到1到n之间的全部质数
    
    if (!(is.numeric(n) && n > 1 && n %% 1 == 0)) {
        stop("请输入大于1的整数")
    }
    
    vec <- 2:n
    
    vec[purrr::map_lgl(vec,is_prime_for)]
    
}

# get_prime_map(19)


```

总结，这是一个很基础，但对理解R语言循环编程，以及提取向量元素，存储向量元素等基础操作有帮助。
