---
title: midterm exam
author: Shiguang Wu
date: April 23, 2022
categories: [parallel algorithm, note, exam]
---

## Algorithm 1

### Q1 Compare the work complexity and the span complexity

work complexity 是当处理器个数为 1 时，完成算法所需的基础操作的个数。span complexity 是在数据依赖 DAG 图中的最长路径的时间复杂度，即当处理器个数为 $\infty$ 时，基础操作的个数。

### Q2 State and prove the Brent's Theorem

Brent's Theorem:

$$
\max\left\{T_\infty,\frac{T_1}{p}\right\} \le T_p \le \frac{T_1-T_\infty}{p} + T_\infty.
$$

Proof:
左侧不等式显然。

对于右侧不等式，

对 DAG 图进行分层，记第 $i$ 层的操作数为 $m_i$，共 $n$ 层，则有
$$
T_1=\sum_{i=1}^n m_i
$$

对第 $i$ 层进行并行，由于这 $m_i$ 个操作独立，所以

$$
T_p^i=\left \lceil \frac{m_i}{p} \right \rceil \le \frac{m_i-1}{p} + 1.
$$

故

$$
T_p=\sum_{i=1}^{n}T_p^i\le \sum_{i=1}^{n}\left ( \frac{m_i-1}{p}+1\right)=\frac{T_1-T_\infty}{p} + T_\infty\blacksquare
$$

### Q3 Associative? Why important

- 可结合性即，$\forall a,b,c$，有 $\text{oper}(\text{oper}(a,b),c)=\text{oper}(a, \text{oper}(b,c))$。

- 由于并行程序在 scheduling 时，每个 task 的开始时间和运行时间我们无法预知，对于一系列的操作无法保证运算的顺序，从而保证正确性。若操作满足可结合性，我们可以使用 merge 等算法保证操作数顺序的同时进行并行处理；若操作又满足可交换性，则可以简单的使用 par-for 等进行并行，且能保证正确性。

### Q4 Compare array scan and list ranking

difference

- scan 操作的对象是一个数组，每一个位置的答案所对应的依赖元素是 predefined。list ranking 操作对象是 linked list 或 array pool，每个位置的答案所依赖的元素是未知的，必须经过某种串行的查找来得到对应的依赖关系。

- scan 计算的是 $a_i=\bigoplus_{k=1}^i\text{arr}_k$，list ranking 计算的是每个 element 的 ranking，其中 $\bigoplus$ 是可结合、可交换的二元运算。

- list ranking 使用 jump 的思想来解决，而 scan 没有。

in common

- 两种算法解决的问题都是 $n\to n$。

### Q5 Compare quicksort and samplesort

<!-- TODO -->

difference

- quicksort 只进行一次采样，选择一个 pivot element，将数组分成两段，而 samplesort 会进行 $k\cdot (p-1)$ 次采样，选出 p-1 个 pivot element，将数组分成 p 段。

in common

- 都是基于 divide and conquer 思想，解决 sorting 问题

## OpenMP 1

### Q6 False sharing and how to avoid

- 由于 cache 的存在，在一个 cpu 上的 cache 中可能会有其他 cpu 所处理的与当前 cpu 无关的数据 (一般是数组元素)，当那一部分的数据发生更改时，当前处理器的 cache 需要进行同步，从而进行一系列的没有意义的 IO 操作，从而严重影响并行性能。

- 避免 false sharing

  1. 对于数组，我们可以通过增加 padding，强制将不同 cpu cache 里的元素隔离开。
  
  2. 避免使用全局数组，对每个 task 使用 private 的数组，使用 synchronization pragma 来进行同步。

### Q7 Why do synchronizations slow down your program 

- 若有很多线程同时需要进行同步操作，进入排队等待状态，那么此时并行退化成了串行，会造成性能的下降。

- 同步操作本身会有一定的开销，如排队调度、处理器之间的通信等。

### Q8 When should we use critical, atomic or lock pragma

- critical: 当需要避免 data racing，只允许一个线程进入，而且关键区计算的开销不是非常大的时候使用。

- atomic: 只能在一些简单的运算上使用(需要硬件支持)，如 `a++` 等。

- lock: 当需要更复杂的同步或有复杂的锁的关系时使用。

### Q9  Give four reasons why the iterations are not in a static order for a parallel for loop

1. parallel for 的 schedule 参数默认是 static，即在循环开始前为每个 thread 分配大致等量个 iterations[^1]。因此，由于不同 thread 执行的 iterations chuncks 之间在运行时是独立的，它们执行的顺序是不确定的。

2. 程序的运行会因为 cpu 的调度等原因，导致运行的时间不固定。从而影响执行的顺序。

3. 计算机并不能保证提供固定数量的 cpu，因此 OpenMP 对并行 tasks 的调度和管理也不固定。

4. 由于 OpenMP 和操作系统的调度，threads 的执行顺序并不一定是他们的创建顺序，且不固定。

::: {.callout-note}
## solution
(Compile, Run) $\times$ (Hardware, Software)
:::

### Q10 Safe way to generate random numbers in parallel

使用并行版 LCG 算法生成随机数。

---

```c++
// 原 LCG 算法
int random() {
    random_next = (MULTIPLIER * random_last + ADDEND) % PMOD;
    random_last = random_next;

    return random_next;
}
```

---

在原 LCG 算法基础上，使用 Leap Frog Method。记由 LCG 算法生成的随机数序列为 $a_0,a_1,\dots$，有 k 个 thread。取 $a_0,a_1,\dots,a_{k-1}$ 作为每个 thread 随机数序列的种子，并修改 `MULTIPLIER` 和 `ADDEND`，使得 $\text{next}(a_i)=a_{i+k}$，即每个 thread $t$ 所使用的随机数序列为 $a_{t::k}$。

## Algorithm 2: comment deletion

solution: scan, fragsum

## Algorithm 3: missing element

solution: xor

## Algorithm 4: KNN

solution:

```c++
for i: 0 to N
    for j: i+1 to N
        d[i,j]=dis[A[i],A[j]] // heap[i].add, heap[j].add, lock
```

designed schedule to balance work flow in each thread

lock

[^1]: Openmp.org. [Online]. Available: <https://www.openmp.org/wp-content/uploads/OpenMP-API-Specification-5-1.pdf>. [Accessed: 22-Apr-2022].
