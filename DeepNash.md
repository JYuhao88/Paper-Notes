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


## Policy Gradient $\to$ FoReL

Policy Gradient 的优化目标 
$$\argmax_{\bar{\pi}^i} \mathbb{E}_{h \in \mathcal{H}} (V^i) - \phi(\bar{\pi}^i)$$

其中 
$$\mathbb{E}_{h \in \mathcal{H}} (V^i) 
= \sum_{a \in \mathcal{A}, h \in \mathcal{H}} Q^i(h, a) \bar{\pi}^i (a | h) \rho^{\bar{\pi}^i, \pi^{-i}}(h) \\
= \sum_{a \in \mathcal{A}, h \in \mathcal{H}} Q^i(h, a) \bar{\pi}^i (a | h) \rho^{\bar{\pi}^i}(h) \rho^{\pi^{-i}}(h) $$

负熵 
$$ \phi(\bar{\pi}^i) =  \bar{\pi}^i \log  \bar{\pi}^i $$

### Following the Regularized Leader
离散形式
$$
\pi_t = \argmax_{\pi' \in \Delta^{|\mathcal{A}|}} [\pi' \cdot y_{t-1} - \phi(\pi')], \quad y_t = y_{t-1} + \eta_t u_t,
$$
其中 
注：
$$ L = \pi' \cdot y_{t-1} - \phi(\pi') $$
$$ \frac{\partial L}{\partial \pi'} = y_{t-1}^T - \log \pi' - |\mathcal{A}|$$

连续形式


<!-- 新的优化目标：
$$
\bar{J}(y) = \sum_{i=1}^N \sum
$$ -->
<!-- (information state $x \in \mathcal{X} = \bigcup_{i \in \{ 1, \dots, N, c\}} \mathcal{X}_i$) -->


## CFR

假设在第 $t$ 次迭代中，各参与者的行为策略 $\sigma^t$ 已经给定。一次CFR迭代的计算流程包括以下几步：

计算本次迭代的regret：
$$
R_t(I,a)=\sum_{h \in I} \pi^{\sigma^t}_p (h)[u^{\sigma^t}(h,a)−u^{\sigma^t}(h)]. 
$$
在实际计算中，通常通过深度优先搜索（DFS）加递归的方式遍历所有 $h$ 并更新 $r_t(I,a)$：
$$
R_t(I(h),a) \leftarrow R_t(I(h),a) + \pi^{\sigma^t}_{-p} [u^{\sigma^t}(h\cdot a)−u^{\sigma^t}(h)].
$$
这种计算方式的好处是，可以在前向传播（即 $h+a \to h′$）的过程中迭代计算概率 $\pi^{\sigma^t}_{-p}(h)$，在后向传播（即 $h' \to (h,a)$）的过程中迭代计算 $u(h)$ 和 $r(I,a)$ ，这样通过一次DFS就能完成全部计算，节省计算量。
更新累计regret：
$$R_t(I,a)=R_{t−1}(I,a)+r_t(I,a).$$
根据累计regret做regret matching，产生下一次迭代的行为策略：
$$\sigma^{t+1}_p(I,a) = \frac{R_t^+(I, a)}{\sum_a R_t^+(I, a)},$$
其中 $R^+_t(I,a) = \max(R_t(I,a),0)$.
在完成整个（多步）CFR迭代流程后，我们进一步对此前各步策略取平均（average policy），作为最终产出的策略形式：
$$
\bar{\sigma}^T (I, a) = \frac{\sum_{t \leq T} \sum_{h \in I} \pi_p^{\sigma^t}(h) \cdot \sigma^t_p(I, a)}{\sum_{t \leq T} \sum_{h \in I} \pi_p^{\sigma^t}(h)}
$$

在实际计算中，为了简化计算量，在迭代过程中我们通常只维护上述表达式中的分子部分 $\sigma^T(I,a)$；当需要用到策略 $\bar{\sigma}^T$ 时，我们只需要进行一步归一化即可：$\bar{\sigma}^{T}(I,a)= \frac{\tilde{\sigma}^T(I,a)}{\sum_{a \in \mathcal{A}} \tilde{\sigma}^T(I,a)}$。
而 $\tilde{\sigma}^T(I,a)$ 同样可以通过迭代的方式进行更新：具体来说，在上面的单步 CFR 迭代流程中，在第1步遍历h计算当次迭代的 regret 时，我们同时计算平均策略的变动量
$$
\Delta \tilde{\sigma}^t(I,a) \leftarrow \Delta \tilde{\sigma}^t(I,a) + \pi_p^{\sigma^t}(h) \cdot \sigma^t_p(I, a);
$$
然后在第2步更新累计regret时，我们同时更新平均策略
$$
\tilde{\sigma}^t(I, a)= \tilde{\sigma}^{t-1}(I, a)+ \Delta \tilde{\sigma}^t(I, a).
$$
在这一小节的最后，我们简单地分析CFR这一算法的计算复杂度。首先，在第1步的计算中，需要遍历所有h的所有动作选项a，因此时间复杂度是O(|H|⋅|A|)；而空间复杂度则是O(|I|⋅|A|)。而在第2和第3步的计算中，时间和空间复杂度都是O(|I|⋅|A|)。因此，算法的整体时间复杂度是O(|H|⋅|A|⋅T)，空间复杂度是O(|I|⋅|A|⋅T)，其中T是算法的迭代步数。可以看出，当问题的状态空间H和可观测空间I很大时，算法在计算上是不太可行的。

## Replicator Dynamics







[1] https://hanqiu92.github.io/blogs/2021/CFR_202103/