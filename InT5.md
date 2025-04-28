# 信息论笔记第3讲——无损压缩

这一章主要考虑在**零失真**情况下的信息压缩与编码——通过数学推导以达到最小化码长的优化目标，并推导我们在数据结构中学习过的哈夫曼编码。

## 1. 建模
对于DMS \( S \) 和失真度量 \( d \)，当 \( D = 0 \) 时，率失真函数为
  \[
  R(0) = H(S),
  \]
  其中 \( H(S) \) 是信源的熵。当 \( n \to \infty \)时，渐进无损压缩的极限也是 \( H(S) \)，但无法保证完全无失真。

## 2. 变长编码
考虑编码集合
\[
    \mathcal{W}^*=\{\empty,0,1,00,01,10,11,000,001,\cdots\},
\]
不失一般性地假设对DMS \( S \)，其中\(\mathcal{S}=\{a_1,a_2,\cdots\}\) ，有\(P(a_1)\geq P(a_2)\geq\cdots\)，则设计编码方案为\(f(a_1)=\empty,f(a_2)=0,f(a_3)=1,\)etc.，那么，第$i$个取值的码长将满足\( \ell(a_i) = \lfloor \log_2 i \rfloor \)。

据此可以分析平均码长
\[
    \bar{\ell}=\sum_i P(a_i)\ell(a_i)
\]
的上下界。
- 上界
   对$a_i$，码长$\ell(a_i) = \lfloor \log_2 i \rfloor \leq\log_2 i\leq -\log_2 P_S(a_i)$（因$P_S(a_i) \leq 1/i$），代入期望码长：
   $$\begin{aligned}
   \bar{\ell} &= \sum_{i} P_S(a_i) \ell(a_i) \\
   &\leq -\sum_{i} P_S(a_i) \log_2 P_S(a_i) \\
   &= H(S).
   \end{aligned}$$
- 下界
引入码长随机变量$\ell(S)$，给定期望$\bar{\ell}$，利用链式法则有
   $$\begin{aligned}
   H(S) &= H(S|\ell(S)) + H(\ell(S)) \\
   &\leq \bar{\ell} + \log_2 [\mathrm e(H(S)+1)],
   \end{aligned}$$
第一部分的放大是由于每个码长对应最多$2^\ell$个符号，第二部分是给定期望的熵上界（见第1讲）
$$\begin{aligned}
    H(\ell(S))&\leq (\bar{\ell}+1)\log(\bar{\ell}+1)-\bar{\ell}\log\bar{\ell}\\
    &<\log_2 [e(\bar{\ell}+1)]\\
    &\leq \log_2 [e(H(S)+1)],
\end{aligned}$$
因此
   $$ H(S) - \log_2 [e(H(S)+1)] < \bar{\ell}. $$
考虑到率$R=\frac{\bar{\ell}}{n}$，上式在$n\rightarrow\infty$时渐进地证明了$R=H(S)$。

## 3. 唯一可译码与Kraft不等式

先给出如下的定义

- 对于DMS $S$，其$q$进制编码$W = f(S)$是一个有限长度字符串，取自集合：
$$\{0,1,...,q-1\}^* := \bigcup_{n=1}^\infty \{0,1,...,q-1\}^n,$$
编码长度记为$\ell(S)$，映射$f:S \mapsto \{0,1,...,q-1\}^*$称为$S$的**基编码**。

- 对于任意有限长度信源消息$\underline{s}=[s_1,...,s_n]$，定义**扩展编码**：
$$f(\underline{s}) = [f(s_1),...,f(s_n)],$$
i.e. 将$f$扩展到所有有限长度信源消息$S^*$。


- 编码$f$是**非奇异的**，如果它在基编码意义下是单射，即：
$$\forall s \neq s' \in S, f(s) \neq f(s')$$


- 编码$f$是**唯一可译的**，如果在扩展编码意义下是单射，即：
$$\forall \underline{s} \neq \underline{s}' \in S^*, f(\underline{s}) \neq f(\underline{s}')$$

- 编码$f$是**前缀自由的**，如果：
$$\forall s \neq s' \in S, f(s)不是f(s')的前缀$$

那么有如下（显然）的引理

- **引理1**：任何前缀自由码都是唯一可译码，但反之不成立。

一个直接的设计是：任意由$q$个符号组成的前缀自由码，可以看作是一个$q$叉树的叶子节点（参考哈夫曼树）。其码字就是搜索这棵树的路径编码，编码长度即为树深。基于这个思路，我们有以下优美的结论。

- **定理1**：对于*有限*字母表DMS $S$的任何**前缀自由码**，必须满足：
$$\sum_{s \in S} q^{-\ell(s)} \leq 1,$$
其中$q$为进制基数。反之，对任何满足该不等式的码长集合，都存在对应的**前缀自由码**。
证明：
    - 必要性：
    将前缀码视为$q$叉树，叶子节点对应码字；设最大码长为$\ell_{\max}$，每个码字$s$的深度为$\ell(s)$。对任意码字$s$，将其叶子完全扩展树到深度$\ell_{\max}$时，可以生成$q^{\ell_{\max}-l(s)}$个叶子节点。根据前缀自由码的性质，新生成的叶子节点不会重叠，即
       $$\sum_{s} q^{\ell_{\max}-\ell(s)} \leq q^{\ell_{\max}},$$
    两边除以$q^{\ell_{\max}}$得证。

    - 充分性
    将码长按非递减排序$\ell(a_1) \leq \ell(a_2) \leq \cdots \leq \ell(a_{|\mathcal{S}|})=\ell_{\max}$。从满$q$叉树的根开始，依次为第$i$个码字分配第一个未占用的，深度为$\ell(a_i)$的节点，并剪除其所有叶子节点（在最深一层，有$q^{\ell_{\max}-\ell(a_i)}$个）。因$\sum_i q^{-\ell(a_i)} \leq 1$，可得$\sum_{s} q^{\ell_{\max}-\ell(s)} \leq q^{\ell_{\max}}$，因此总能找到足够的节点。

进一步地，有
- **定理2**：任何唯一可译码的码长$\{\ell(s)\}$也满足Kraft不等式

- 证明：考虑所有长度为$k$的源消息$\underline{s} \in S^k$的码字拼接
   $$\left( \sum_{s \in S} q^{-\ell(s)} \right)^k = \sum_{m=k}^{k\ell_{\max}} A(m) q^{-m},$$
其中$A(m)$表示源消息长度为$k$，且编码长度为$m$的消息。唯一可译性要求$A(m) \leq q^m$，否则发生编码冲突，故：
   $$\left( \sum_{s \in S} q^{-\ell(s)} \right)^k \leq k(\ell_{\max} - 1).$$
令$k \to \infty$，即得
   $$\sum_{s \in S} q^{-\ell(s)} \leq[k(\ell_{\max}-1)]^\frac{1}{k}\rightarrow 1.$$

定理2告诉我们，任何唯一可译码均可转换为等码长的**前缀自由码**，无性能损失，我们只需研究后者即可



## 4. 最优变长编码
- **Shannon-Fano编码**
  - 码长分配：\( \ell(s) = \lceil -\log_q P_S(s) \rceil \)。
  - 期望：\( H(S) \leq \bar{\ell} < H(S) + 1 \)。
    证明略，取$P_S$和$q^{l(s)}$的softmax分布的KL散度即证。值得注意的是，这种编码效率并不高，例如参数为$\frac{1}{16}$的二项分布，码长分别为1和4，显然非常浪费。
- **Huffman编码**
  Huffman编码倾向于生成概率上“平衡”的编码树
  - 构造方法（参考数据结构）：
    1. 将符号按概率降序排列。
    2. 合并概率最小的两个符号，生成新节点，其概率为两者之和。
    3. 递归直至只剩一个根节点。
  
  在给定概率分布下，可以证明（归纳地）Huffman码的 \( \bar{\ell} \) 最小。推广到 \( q \)-进制情况时，需调整合并策略为：每次合并 \( q-r \) 个符号，其中$r$为$q-1$除$(K-q)(q-2)$的余数。


## 5. 习题
1. 计算\(\{1,2,\cdots,M\}\)上的均匀分布的平均码长。
2. 证明：二进制下的Kraft不等式取等，当且仅当采用了Huffman编码。并由此说明，两个不同字母表上的Huffman编码$f(s)$和$g(t)$拼接后的编码$fg(s,t)$仍为Huffman编码。
3. 对均匀分布信源 \( \{1, \ldots, 10000\} \)，计算Huffman码的 \( \bar{\ell} \) 并与熵比较。
4. 参考T. M. Cover and J. A. Thomas. Elements of Information Theory, 2nd ed. John Wiley & Sons, Hoboken, NJ, USA, 2006，证明无穷字母表下的Kraft不等式。
