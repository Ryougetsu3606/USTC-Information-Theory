# 信息论笔记第4讲——信道传输的容量-成本
乍一看与第2讲的内容十分接近，构造思路也类似。从课后习题看这一章更重视计算，而证明部分被简化，因此本笔记也会在内容上有所侧重。

## 1. 建模
第2讲中我们已经给出如何对信源 **编码** 和 **还原**，然而中间还需要经过一个（有噪声）的信道。本章先对信道的 **容量** 进行建模。

考虑离散无记忆信道（Discrete Memoryless Channel, DMC）$P_{Y|X}$，它应将信源编码的索引$w$转化为一串信号$\underline{x}$，称为信道码字。本章考虑信道索引为均匀分布的情况。类似于第2讲，信道传输的过程可以用马尔科夫链表示：
$$
    W\leftrightarrow \underline{X}=f_n^c(W)\leftrightarrow \underline{Y}\leftrightarrow \hat{W}=g_n^c(\underline{Y}).
$$

接下来仿照率$R$，定义信道容量$C$
- **定义1** 对DMC，若存在信道编码解码器对满足：
$$
    \begin{align}
        R&=\frac{\lceil\log_2 M_n\rceil}{n},\\
        \lim_{n\rightarrow\infty} P_{e,\max}&=0,
    \end{align}
$$
其中$P_{e,\max}$为传输出错的概率$\max P(\hat{W}\neq w|W=w)$，则称速率$R$可达。一个DMC的容量$C$是所有可达速率的上确界。

对每个码字$\underline{x}$，我们定义其 **成本**
$$
    c(\underline{x})=\frac{1}{n}\sum_{i=1}^n c(x_i).
$$
- **定义2** 一般地，我们定义一个DMC的容量成本函数$C(\Gamma)$为下列优化问题的解
$$
    \begin{align}
        \max R &=\frac{\lceil\log_2 M_n\rceil}{n},\\
        \text{s.t. }c(f_n^c(w))&\leq \Gamma,\forall w\\
        \lim_{n\rightarrow\infty} P_{e,\max}&=0,
    \end{align}
$$
这同样是渐进地。

## 2. 香农基本定理——信道编码及其求解
类似第2讲，定义信息容量成本函数$C_I$为下列优化问题的解：给定DMC，即一个分布$P_{Y|X}$，
$$
    \begin{align}
        \max I(X;Y),\\
        \text{s.t. }\mathbb{E}[c(X)]\leq \Gamma,
    \end{align}    
$$
那么$C(\Gamma)=C_I(\Gamma)$

- **推论1** 对信息容量成本函数
  1. 对$\Gamma<\Gamma_{\min}=\min c(x)$，优化问题(6)(7)不可行。
  2. 存在上界$\Gamma_{\max}$使得$C_I(\Gamma)=C_I(\infty)$，在$\Gamma\geq\Gamma_{\max}$时。
  3. $C_I(\Gamma)$关于$\Gamma$凹单调非降。
  4. 若$C_I(\infty)>0$，则$C_I(\Gamma)$在定义域内严格递增，且不等式约束可以变为等式。
   ---
- **证明**：
  1. 由 \(\Gamma_{\min} = \min_x c(x)\)，当 \(\Gamma < \Gamma_{\min}\) 时约束 \(\mathbb{E}[c(X)] \leq \Gamma\) 无可行解，根据定义，此时 \( C_I(\Gamma) = 0 \)
  2. 设无约束容量 \( C_I(\infty) = \max_{P_X} I(X;Y) \)。由于 \( I(X;Y) \) 有界（\( \leq H(Y) \leq \log|\mathcal{Y}| \)）。对 \(\Gamma \geq \Gamma_{\max}\)，约束 \(\mathbb{E}[c(X)] \leq \Gamma\) 不改变可行集
  3. 设 \(\Gamma_1 < \Gamma_2 \leq \Gamma_{\max}\)。设 \( P_X^{(1)} \) 为 \(\Gamma_1\) 的最优分布，则：
   \[ \mathbb{E}_{P_X^{(1)}}[c(X)] \leq \Gamma_1 < \Gamma_2, \]
   故 \( P_X^{(1)} \) 也是 \(\Gamma_2\) 的可行解。但 \( C_I(\Gamma_2) \) 可能通过其他分布取得更大值：若存在 \( P_X^{(2)} \) 使 \(\mathbb{E}[c(X)] \in (\Gamma_1, \Gamma_2]\) 且 \( I(X;Y) > C_I(\Gamma_1) \)，则由互信息的连续性得证严格递增性。
   设 \(\Gamma = \lambda \Gamma_1 + (1-\lambda)\Gamma_2\)，\(\lambda \in (0,1)\)。取 \( P_X^{(\lambda)} = \lambda P_X^{(1)} + (1-\lambda)P_X^{(2)} \)，其中 \( P_X^{(i)} \) 为 \(\Gamma_i\) 的最优分布，则由期望的线性性和互信息 \( I(X;Y) \) 关于 \( P_X \) 的凹性（第1讲第5节），有
    $$
    \begin{aligned}
    \mathbb{E}_{P_X^{(\lambda)}}[c(X)] &= \lambda \Gamma_1 + (1-\lambda)\Gamma_2 \leq \Gamma,\\
    I_{P_X^{(\lambda)}}(X;Y) &\geq \lambda C_I(\Gamma_1) + (1-\lambda)C_I(\Gamma_2),
   \end{aligned}
    $$
    因此：
   \[
   C_I(\Gamma) \geq I_{P_X^{(\lambda)}}(X;Y) \geq \lambda C_I(\Gamma_1) + (1-\lambda)C_I(\Gamma_2).
   \]
   4. 因\( C_I(\Gamma) \) 的凹性可知其在定义域严格递增（若有“水平”段，则与凹性矛盾），最优解必在边界取得，故当 \( C_I(\infty) > 0 \) 时，不等式约束可替换为等式。

有算例如下
- **例1** 考虑二进制对称信道（Binary Symmetric Channel, **BSC**），即$\mathcal{X}=\mathcal{Y}=\{0,1\}$，且
$$
    P_{Y|X}=\begin{bmatrix}
        1-\delta&\delta\\\delta&1-\delta
    \end{bmatrix},
$$
在$H(Y|X)$已知的前提下，可以方便地展开互信息，即
$$
    \begin{aligned}
        I(X;Y)&=H(Y)-H(Y|X)\\
        &=H(Y)- \sum_{x=0,1} P_X(x)H(Y|X = x)\\
        &=H(Y) - \sum_{x=0,1} P_X(x)h_2(\delta)\\
        &=H(Y) - h_2(\delta)\\
        &\leq 1 - h_2(\delta),
    \end{aligned}
$$
等号成立当且仅当$Y$服从参数为$\frac{1}{2}$的伯努利分布，从而$X$也服从参数为$\frac{1}{2}$的伯努利分布，此时$C=1-h_2(\delta)$。

- **例2** 考虑二进制擦除信道（Binary Erasure Channel, **BEC**），即$\mathcal{X}=\{0,1\},\mathcal{Y}=\{0,1,e\}$，且
$$
    P_{Y|X}=\begin{bmatrix}
        1-\delta&0&\delta\\0&1-\delta&\delta
    \end{bmatrix},
$$
该信道不会出错，但可能将输入擦除为$e$。这里$P_Y$并不容易计算，因此反而假设$P_X(1)=p$，则$P_Y(0) = (1 - p)(1 - \delta), P_Y(1) = p(1 - \delta), P_Y(e) = \delta$，后验概率满足
\[P_{X|Y}(0|0) = 1 , P_{X|Y}(1|0) = 0, \]

\[P_{X|Y}(0|1) = 0 , P_{X|Y}(1|1) = 1,\]

\[P_{X|Y}(0|e) = 1 - p , P_{X|Y}(1|e) = p. \]

那么
$$
    \begin{aligned}
        I(X;Y)&=H(X) - H(X|Y)\\
        &=h_2(p) - [(1 - p)(1 - \delta)h_2(0) + p(1 - \delta)h_2(1) + \delta h_2(p)]\\
        &=(1 - \delta)h_2(p)\\
        &\leq 1 - \delta,
    \end{aligned}
$$
等号成立当且仅当$X$服从参数为$\frac{1}{2}$的伯努利分布，此时$C=1-\delta$。

## 3.两种容量成本函数的等价性
### Converse Part
这一节与第2讲相关内容完全类似，我们将证明\(C(\Gamma) \leq C_I(\Gamma)\)，即对于任何满足成本约束\(\Gamma\)的信道编码器/解码器对，当\(R > C_I(\Gamma)\)时，\(P_{e,\max}\)不可能随\(n \to \infty\)趋近于零。

对马尔科夫链$W\leftrightarrow X\leftrightarrow Y\leftrightarrow \hat{W}$，据Fano不等式，有
$$
    \begin{equation}
        H(W|\hat{W})\leq h_2(P_{e,\text{ave}})+P{e,\text{ave}} \log(M_n - 1) \leq 1 + P_{e,\text{ave}} nR.
    \end{equation}
$$
考虑互信息，由条件降熵与DMC的无记忆性，有
$$
    \begin{aligned}
        I(W;Y)&=H(Y) - H(Y|W)\\
        &=H(Y) - \sum_{i=1}^n H(Y_i|W, Y_{i-1}, \ldots, Y_1)\\
        &\leq H(Y) - \sum_{i=1}^n H(Y_i|W, Y_{i-1}, \ldots, Y_1, X_i)\\
        &= H(Y) - \sum_{i=1}^n H(Y_i|X_i)\\
        &\leq \sum_{i=1}^n H(Y_i) - \sum_{i=1}^n H(Y_i|X_i)\\
        &=\sum_{i=1}^n I(X_i; Y_i),
    \end{aligned}
$$
引入\(\{1, 2, \ldots, n\}\)上的均匀分布$Q$，则
$$
    \begin{aligned}
        \frac{1}{n}I(W;Y)&\leq \frac{1}{n} \sum_{i=1}^n I(X_i; Y_i)\\
        &= I(X_Q; Y_Q|Q)\\
        &= H(Y_Q|Q) - H(Y_Q|X_Q, Q)\\
        &= H(Y_Q|Q) - H(Y_Q|X_Q)\\
        &\leq H(Y_Q) - H(Y_Q|X_Q)\\
        &= I(X_Q; Y_Q).
    \end{aligned}
$$
考虑成本约束
\[\frac{1}{n} \sum_{i=1}^n c(X_i) \leq \Gamma,\]
亦即
\[\mathbb{E}[c(X_Q)] \leq \Gamma,\]
因此$X_Q$是可行的(feasible)，进而
\[\frac{1}{n} I(W; Y) \leq I(X_Q; Y_Q)\leq C_I(\Gamma).\]
综上所述，
$$
    \begin{aligned}
        nR&=H(W)\\
        &= H(W|\hat{W}) + I(W; \hat{W})\\
        &\overset{\text{Fano's}}{\leq}1 + P_{e,\text{ave}} nR + I(W; \hat{W})\\
        &\overset{\text{Markov}}{\leq }1 + P_{e,\text{ave}} nR + I(W; Y)\\
        &\leq 1 + P_{e,\text{ave}} nR + n C_I(\Gamma),
    \end{aligned}
$$
渐进地，对任意$R>C_I(\Gamma)$，当$n \to \infty$时
\[P_{e,\max} \geq P_{e,\text{ave}} \geq 1 - \frac{C_I(\Gamma)}{R} > 0. \]
即证。

### Achievability Part
这一节将证明的是$C(\Gamma)\geq C_I(\Gamma)$，这被转化为：给定\(\Gamma\)，对于任意速率\(R < C_I(\Gamma)\)，存在信道编码器/解码器对序列（索引为码字长度\(n = 1, 2, \ldots\)），使得\(R\)按定义1可达。
此处同样引入码本$\text{C}$，即$\text{C}(w)=f_n^c(w), w=1,\cdots,=M_n$，且码字长度均为$n$。平均错误率由下式给出，我们考虑其期望
\[P_{e,\text{ave}} = \frac{1}{M_n} \sum_{w=1}^{M_n} P(g^{(c)}_n(Y) \neq w | X = C(w)).\]
- 码本生成规则：给定约束的$P_X$和$R$，码本的生成是对$\mathcal{X}^n$的$M_n$次采样，采样到每个码字的概率显然服从$\Pi_{i=1}^n P_{X_i}$。
- 解码规则：给定$\epsilon > 0$和$Y=y$，寻找索引$w$使之满足：
$$
    \begin{align}
        \frac{1}{n}\sum_{i=1}^n c(\text{C}_i(w))&:=c(\text{C}(w))\leq \Gamma+\epsilon,\\
        I(X;Y)-\epsilon\leq \frac{1}{n}\sum_{i=1}^n\log_2\frac{P(y_i|\text{C}_i(w))}{P(y_i)}&:=i(\text{C}(w);y)\leq I(X;Y)+\epsilon,
    \end{align}
$$
若找不到则设为1。后略，感觉不会考。
## 4. 联合源-信道编码
对DMS和DMC，使用源-信道联合的编码方式（即把他们放在同一个函数里）是一个更优的编码方案（比起分离编码）。给定失真和容量约束对$(D,\Gamma)$，联合编码可以达到$R(D)=C(\Gamma)$，分离编码需满足$R<C$。
## 5. 习题
1. 若DMC的转移矩阵的每一行和列有相同元素集合，求其容量，并证明其容量在输入为均匀分布时取到。
2. 不允许**失真**的情形下，对5个字母的信源，证明其速率为\(\frac{1}{2}\log 5\)。