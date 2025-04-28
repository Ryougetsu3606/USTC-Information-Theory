# 信息论笔记第2讲——率失真理论

这一章主要考虑信息的 **压缩**(compression) 传递，在允许 **失真**(distortion) 的前提下，尽可能地增大 **率**(rate)。对记号作说明：小写表具体值，大写表随机变量，花体表字母表，下划线表序列，戴帽表接收端信息
## 1.建模

考虑离散无记忆信源(discrete memoryless source, DMS)随机变量
$$
    \underline{S}=[S_1,\cdots,S_n],
$$
即$S_1,\cdots,S_n$是i.i.d.的随机变量。记其pmf为 $P_S(s)$，字母表为$\mathcal{S}$。分别定义

- 编码器(encoder)
$$
    f_n^{(s)}: \mathcal{S}^n\mapsto\{1,2,\cdots,M_n\},
$$
把一串信息$\underline{S}$编码成索引$W$。

- 解码器(decoder)
$$
    g_n^{(s)}: \{1,2,\cdots,M_n\}\mapsto\mathcal{\hat{S}}^n,
$$
把索引$W$还原为信息$\underline{\hat{S}}$。

整个信息传递的过程就是：信源通过编码成索引，再通过解码成接收信息。
- 注意：事实上，编码器和解码器是两个条件概率分布；发送方和接收方的字母表有可能不同。
------
接下来给出**率**和**失真**的定义。
- 率
$$
    R=\frac{\lceil\log_2M_n\rceil}{n},
$$
衡量信息传输的效率，率越小，说明用越少的索引就可以表示信息。但小的率依赖于压缩，因此必然带来——
- 失真
对两个信息$s$和$\hat s$，用$d(s,\hat s)$表示两者间由压缩引起的 **失真** ，这里认为失真是**有界**的。顾名思义，偏差越大的信息失真值应越大。对一串信息，其失真定义为各个失真的算术平均值。常见的失真定义为汉明失真(Hamming distortion):
$$
    d(s,\hat{s})=\begin{cases}
        1,s\neq\hat s,\\
        0,s=\hat s.
    \end{cases}
$$

我们直接定义率失真函数$R(D)$ (rate-distortion function)，它表示在给定（期望）失真下，率的 **下界**。即
$$
    \begin{aligned}
        \min R&=\frac{\lceil\log_2M_n\rceil}{n},\\
        \text{s.t. }\lim_{n\rightarrow\infty}\mathbb{E}[d(\underline{S},\hat{\underline{S}})]&\leq D.
    \end{aligned}
$$

## 2.率失真函数的求解与性质
- 互信息率失真函数
定义以下的优化问题：
$$
    \begin{aligned}
        \min_{P_{\hat S|S}} R_I&=I(S;\hat S),\\
        \text{s.t. }\mathbb{E}[d(\underline{S},\hat{\underline{S}})]&\leq D,
    \end{aligned}
$$
其最优解$R_I(D)$称为信息率失真函数，那么，给定DMS和失真，有
$$
    R(D)=R_I(D).
$$
证明见后。

上面的约束实际上限制了$P_{\hat S|S}$的选择——这一期望在给定了pmf和失真（函数）的情况下，只与条件分布$P_{\hat S|S}$有关。
----- --
- 率失真函数的计算
  率失真函数的计算步骤概括如下。
  1. 确定定义域
    定义域由以下算法确定
    $$
        D_{\min}=\sum_s P_S(s)\min_{\hat{s}}d(s,\hat{s}),
    $$
    $$
        D_{\max}=\min_{\hat{s}}\sum_s P_S(s)d(s,\hat{s}),
    $$
    容易验证若$D<D_{\min}$，无论如何选取条件分布约束都无法满足；若$D>D_{\max}$，可以选取固定的$\hat S$恒为$\hat s^*$使得互信息为0。

   2. 对互信息进行放缩
    实际情况下，约束项$\mathbb{E}[d(\underline{S},\hat{\underline{S}})]\leq D$的小于号可以去掉，保留为等式。这里用一个例子来说明。给定参数为$\delta$的二项分布，如此计算
    $$
    \begin{aligned}
        I(S;\hat S)&=H(S)-H(S|\hat{S})\\
        &=h_2(\delta)-H(S\oplus \hat{S}|\hat{S})\\
        &\geq h_2(\delta)-H(S\oplus \hat{S})\\
        &\geq h_2(\delta)-h_2(D).
    \end{aligned}
    $$
    依据分别为：互信息定义、双射函数、条件降熵、约束项等价变换，细节请读者自行思考。
    实际解题时，这种找参考分布的trick不一定work。这时可以直接把$P_{\hat{S}|S}$的各项（有$|\mathcal{S}|\times|\hat{\mathcal{S}}|$项）视作自由变量，在期望约束下进行运筹学意义下优化。

--- ---
- 率失真函数的性质
  - 单调性：互信息率失真函数在定义域上单调递减，因为$D$的增大意味着可行解范围的变大，因而优化目标（互信息）的下界是递减的。
  - 凸性：互信息率失真函数在其定义域上是凸函数，即给定$D_0,D_1\in[D_{\min},D_{\max}]$和$0\leq\lambda\leq 1$，
  $$
        R_I((1-\lambda)D_0+\lambda D_1)\leq (1-\lambda)R_{I}(D_0)+\lambda R_I(D_1),
  $$
  从pmf的凸组合出发，据定义即证。由凸性还可以得出其单调递减是严格的。

## 3.两种率失真函数的等价性
我们从两方面说明$R(D)=R_I(D)$的确成立：逆定理(converse)和可达性(achievability)。首先证明逆定理$R(D)\geq R_I(D)$
### Converse Part
据率的定义
$$
    \begin{aligned}
        nR&= \lceil\log_2M_n\rceil\\
        &\geq H(W)\\
        &\geq I(\underline{S};W)\\
        &\geq I(\underline S;\underline{\hat{S}})\\
        &=H(\underline S)\\
        &=\sum_{i=1}^n H(S_i) - \sum_{i=1}^n H(S_i|\hat S,S_{i-1},\cdots,S_1)\\
        &\geq \sum_{i=1}^n H(S_i)-\sum_{i=1}^n H(S_i|\hat S_i)\\
        &=\sum_{i=1}^n I(S_i;\hat{S}_i),
    \end{aligned}
$$
理由自己想。再引入辅助(auxiliary)的随机变量$Q$，服从1到$n$上的均匀分布，独立于$S$，那么
$$
    \begin{aligned}
        R&\geq \frac{1}{n}\sum_{i=1}^n I(S_i;\hat{S}_i)\\
        &=I(S_Q;\hat{S}_Q|Q)\\
        &=H(S_Q|Q)-H(S_Q|\hat{S}_Q,Q)\\
        &=H(S_Q)-H(S_Q|\hat{S}_Q)\\
        &=I(S_Q;\hat{S}_Q)\\
        &\geq R_I(\mathbb{E}[d(S_Q;\hat{S}_Q)])\\
        &=R_I(\frac{1}{n}\sum_{i=1}^n\mathbb{E}[d(S_i;\hat{S}_i)])\\
        &=R_I(\mathbb{E}[d(\underline{S},\hat{\underline{S}})])\\
        &\geq R_I(D),
    \end{aligned}
$$
理由接着自己想（提示：第二个不等号用到$R_I$的定义）。总之$R(D)\geq R_I(D)$得证。

### Achievability Part
这一Part我们构造性地证明$R(D)\leq R_I(D)$。先证对任意$R> R_I(D)$，总存在一对符合要求的编码/解码对。记前述解码函数$g_n^{(s)}(w)$为一个 **码本**C(codebook)，第$w$个**码字**的第$i$元素记作C$_i(w)$。

考虑两种编码方式，严格地，令
$$
    w^*=\argmin_w d(\underline{s},\text{C}(w)),
$$
且$\underline{\hat{s}}=\text{C}(w^*)$，或渐进地
$$
\begin{aligned}
    d(\underline{{s}},\text{C}(w))&\leq D+\epsilon,\\
    I(S;\hat S)-\epsilon\leq i(\underline{{s}},\text{C}(w))&\leq I(S;\hat S)+\epsilon,
\end{aligned}
$$
其中
$$
\begin{align}
    d(\underline{{s}},\text{C}(w))&=\frac{1}{n}\sum_{i=1}^n d(s_i,\text{C}_i(w)),\\
    i(\underline{{s}},\text{C}(w))&=\frac{1}{n}\sum_{i=1}^n\log_2\frac{P_{S,\hat S}(s_i,\text{C}_i(w))}{P_S(s_i)P_{\hat S}(\text{C}_i(w))},
\end{align}
$$
对给定（？）$\epsilon>0$成立，否则选取$w=1$。(2)式的依据是，如此选出的编码方案倾向于服从分布$P_{S,\hat S}$。接下来分析这种编码方案下的期望失真，令布尔（0-1）变量$E(\text{C},s)$表示编码失败是否发生，那么
$$
    \begin{aligned}
        \mathbb{E}_{\textbf{C},\underline{S}}[d(\underline{S},\hat{\underline{S}})]=&\sum P(\textbf{C})P(\underline{s})d(\underline{{s}},\text{C}(w))\\
        =&\sum_{E(\text{C},s)=0} P(\textbf{C})P(\underline{s})d(\underline{{s}},\text{C}(w))\\
        &+\sum_{E(\text{C},s)=1} P(\textbf{C})P(\underline{s})d(\underline{{s}},\text{C}(1)).
    \end{aligned}
$$
第一部分由式(1)进行放缩，第二部分可放大为$P_fd_{\max}$，其中$P_f$为编码失败的概率，可以写作
$$
    P_f=\sum_{\underline{s}}P(\underline{s})\sum_{\text{C}}P(\text{C})E(\text{C},\underline s).
$$
注意到，对任意$\underline{s}$，$E(\text{C},\underline s)=1$实际意味着所有$M_n$个码字无法同时(simultanously)满足(1)(2)式，又知每个分量是i.i.d.，那么可以进一步改写
$$
    \sum_{\text{C}}P(\text{C})E(\text{C},\underline s)=[1-P((\underline s,\underline{\hat{S}})满足(1)(2)式)]^{M_n}.
$$
又定义$F(\underline{s},\hat{\underline{s}})$是布尔变量，当$(\underline{s},\hat{\underline{s}})$满足(1)(2)式时取1，则
$$
    P((\underline s,\underline{\hat{S}})满足(1)(2)式)=\sum_{\hat{\underline{s}}}P(\hat{\underline{s}})F(\underline{s},\hat{\underline{s}}).
$$
(2)式右端给出
$$
    P(\hat{\underline{s}})\geq 2^{-n(I(S;\hat{S})+\epsilon)}P(\hat{\underline{s}}|\underline{s}).
$$
那么
$$
    \begin{aligned}
        &P((\underline s,\underline{\hat{S}})满足(1)(2)式)\\=&\sum_{\hat{\underline{s}}}P(\hat{\underline{s}})F(\underline{s},\hat{\underline{s}})\\
        \geq& 2^{-n(I(S;\hat{S})+\epsilon)}\sum_{\hat{\underline{s}}}P(\hat{\underline{s}}|\underline{s})F(\underline{s},\hat{\underline{s}}).
    \end{aligned}
$$
综合上式放大$P_f$
$$
    \begin{aligned}
        P_f\leq &\sum_{\underline{s}}P(\underline{s})\left[1-2^{-n(I(S;\hat{S})+\epsilon)}\sum_{\hat{\underline{s}}}P(\hat{\underline{s}}|\underline{s})F(\underline{s},\hat{\underline{s}})\right]^{M_n}\\
        \leq&\sum_{\underline{s}}P(\underline{s})\left[1-\sum_{\hat{\underline{s}}}P(\hat{\underline{s}}|\underline{s})F(\underline{s},\hat{\underline{s}})+\mathrm{e}^{-M_n2^{-n(I(S;\hat{S})+\epsilon)}}\right]\\
        =&1-\sum_{\underline{s},\hat{\underline{s}}}P(\underline{s},\hat{\underline{s}})F(\underline{s},\hat{\underline{s}})+\mathrm{e}^{-M_n2^{-n(I(S;\hat{S})+\epsilon)}},
    \end{aligned}
$$
其中第二个不等式用到$(1-xy)^n\leq 1-x+\text e^{-ny}$。回顾$R$的定义，事实上，上式$\sum_{\underline{s},\hat{\underline{s}}}P(\underline{s},\hat{\underline{s}})F(\underline{s},\hat{\underline{s}})$是$F(\underline{S},\hat{\underline{S}})$的期望，亦即$(\underline{S},\hat{\underline{S}})$满足式(1)(2)的概率，它依大数定律收敛到1。另外，
$$
    \begin{aligned}
        M_n2^{-n(I(S;\hat{S})+\epsilon)}=&2^{nR}2^{-n(I(S;\hat{S})+\epsilon)}\\
        =&2^{n(R-I(S;\hat{S})-\epsilon)},
    \end{aligned}
$$
而我们已经假设$R>I(S;\hat{S})$（本part的开头），那么对足够小的$\epsilon$，$\mathrm{e}^{-M_n2^{-n(I(S;\hat{S})+\epsilon)}}$应随$n$的增大收敛到0。综上所述，$P_f$随$n$的增大收敛到0。

对最初的期望$\mathbb{E}_{\textbf{C},\underline{S}}[d(\underline{S},\hat{\underline{S}})]$，由以上可得，在$n$趋于正无穷时，其值不大于$D+2\epsilon$。因此，令$\epsilon\rightarrow 0$，对于$R>I(S;\hat S)$，存在一个码本C（亦即分布$P_{\hat S|S}$），使得$\mathbb{E}[d(\underline{S},\hat{\underline{S}})]\leq D$，从而$R>I(S;\hat{S})$在$D$下可行。又令$R_I(D)=I(S;\hat{S})$，得证。

说人话就是，对于任意的$R>R_I(D)$，都可以找到编码方案，使之率为$R$，而失真不大于$D$。因此，给定$D$，$R$的下界至少不大于$R_I(D)$，亦即$R(D)\leq R_I(D)$。

## 4.习题
率失真定理的证明思路确实不大好想，特别是可达性部分，有余力还是背下来好。此外，可达性的证明并没有给出一个确切的编码方案，但至少证明了其存在性，收获还是不小的。
1. 计算率失真函数：信源字母表为{-1,0,1}，恢复字母表为{-1,1}，信源服从均匀分布，失真度量为
$$
    \begin{bmatrix}
        0&1\\0&0\\1&0
    \end{bmatrix}
$$

2. 计算在字母表为$\{1,\cdots,m\}$，且服从均匀分布的信源，以及汉明失真下的率失真函数（提示：Fano不等式）。
3. 举例说明若$d_{\max}=\infty$，那么，率失真定理$R(D)=R_I(D)$不一定成立。