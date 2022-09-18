# DeepNash

## Notion

### 拓展式博弈 G 七元组 $< \mathcal{H}, \mathcal{Z}, \mathcal{P}, p, u, \mathcal{I}, \sigma_c >$

$$
\mathcal{P} : \text{所有玩家的集合} \\
h : \text{history, 当前状态的所有信息} \\
\mathcal{H}: \text{状态序列的集合} \\
\mathcal{Z}: \mathcal{Z} \subseteq \mathcal{H}, 终止状态的集合\text{（对应博弈树中的叶子节点）}\\
P: \mathcal{H} \to \mathcal{P}, \text{当前状态（非终止状态）应该由哪位玩家采取行动} \\
\mathcal{A(h)} : \text{the actions available} \\
u: \mathcal{Z} \to \mathbb{R}, 某个玩家到达终止状态z时可以获得的奖励 \\
I_i：\text{玩家 i 的信息集，一个信息集中包含了若干个H中的状态，当其他玩家通过观测历史h时，只知道玩家处于集合I上，但是并不清楚该玩家具体在I的哪一个状态上。可以利用信息集的大小来衡量一个玩家在游戏中隐藏了多少信息} \\
\mathcal{I}_i: 所有I_i 组成的集合 \\
u: \mathcal{Z} \to \mathbb{R}, 某个玩家终止状态 z 时可以获得的奖励 \\
\sigma_{c}: 机会玩家做出所有合法动作的概率分布，可进一步用 \sigma_c(h, a) 来表示当游戏处于状态h时，机会事件a发生的概率
$$

### 策略
策略是玩家在游戏中选择动作的准则，在非完备信息博弈中，策略 $\sigma$ 是一个从当前信息集 I 上所有合法动作到[0,1]之间的一个映射，也可以说 $\sigma$ 是一个基于信息集 I 的概率分布。以下是一些关于策略的常用符号表达：
$$
\sigma_{p}(I, a)：\text{玩家p处于信息集I时，做出动作a的概率} \\
\sigma(I) ：表示一个概率分布，即处于状态集I时，做出所有合法动作的概率分布（将游戏中所有的信息集I上的  组合起来，即可得到完整的策略σ \\
$$

$$
除了策略之外，在博弈问题中经常需要用到一些概率，为了方便表示，我们将定义一系列π概率：\\
\pi^{\sigma}(h) ：从初始状态出发，当所有玩家都遵循策略σ时，到达状态h的概率 \\
\pi_p^{\sigma}(h) ：玩家p从初始状态出发，遵循策略σ，到达状态h的概率 \\
\pi^{\sigma}_{-p}(h)：从初始状态出发，除玩家p外，其他所有玩家都遵循策略σ，到达状态h的概率。
\pi^{\sigma}_{p}(z|h)：玩家p遵循策略σ，从状态h出发，到达状态z的概率。
$$

## Nash Equilibrium
$$
\forall i, u_i(\sigma^*_i, \sigma^*_{-i}) = \max_{\sigma_i'} u_i (\sigma_i', \sigma_{-i}^*)
$$
最佳策略$\sigma^*$：当你的对手用这个策略的时候，你也只能用这个策略。
时

遗憾值 Regret : 
$$
R^T(s) = \sum_{t} s(t) v^t - \sum_t \sigma^t v^t
$$
实际使用的策略 $\sigma$ 替换成策略 s，新策略 s 比原策略多产生的那部分收益，即遗憾值。

## CFR
<!-- $$
\left\{\begin{array}{ll}{50 \frac{\partial u}{\partial t}=\frac{\partial^{2} u}{\partial x^{2}}+2 e^{\frac{t}{50}} \sin (1-x),} & {(x, t) \in \Omega_{T}} \\ {u(x, 0)=\sin (1-x),} & {0 \leq x \leq 1} \\ {u_{x}^{\prime}(0, t)=u(0, t)-e^{\frac{t}{50}}(\sin (1)+\cos (1)),} & {0<t \leq T} \\ {u(1, t)=0,} & {0<t \leq T}\end{array}\right.
$$ -->

$$
\sigma^t_p (I, a) = \left\{\begin{array}{ll}
    R^t(I, a)^+ / \sum_{h \in A(I)} R^t(I, b)^+, & \sum_{b \in A(I)} R^t(I, b)^+ > 0 \\
    \frac{1}{|A(I)|}, & \text{otherwise}
\end{array} \right.
$$

$$
\bar{\sigma_p}^t(I, a) = \frac{1}{t} \sum_{t'=1}^t \pi_p^{}