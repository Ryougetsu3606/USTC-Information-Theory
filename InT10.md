# 信息论笔记第5讲
诸如大模型等场景中，一般认为信息服从某种连续型随机分布。因此我们考虑连续情况下的信息熵
## 1.微分熵

对连续分布$X$，用离散分布$X_\delta$
\[
    P_{X_\delta}(i)=\int_{i\delta}^{(i+1)\delta}f(x)\mathrm{d}x=\delta f(x)
\]
逼近之。后者的熵为
\[
    \begin{aligned}
    H(X_\delta)=&-\sum_i P(i)\log P(i)\\
    =&-\sum_i f(x_i)\delta\log f(x_i)-\sum_i f(x_i)\delta\log \delta\\
    =&-\int_\mathcal{S}f(x)\log f(x)\mathrm{d}x-\delta\log\delta.
    \end{aligned}
\]
因此，我们用
\[
    h(X)=-\int_\mathcal{S}f(x)\log f(x)\mathrm{d}x,
\]
逼近连续型随机变量$X$的熵（若这个积分收敛），称之为微分熵。联合微分熵，条件微分熵的定义照常。
- 均匀分布$U[a,b]$的微分熵为$\log(b-a)$；
- 指数分布$Exp(\lambda)$的微分熵为$1-\log\lambda$；
- 正态分布$N(\mu,\sigma^2)$的微分熵为$\frac{1}{2}\log(2\pi e\sigma^2)$。
- 多维正态分布$N(\bm{\mu},K)$的微分熵为$\frac{k}{2}\log(2\pi e)+\frac{1}{2}\log |K|$。

链式法则和条件降熵的性质保持，互信息、条件互信息等定义相同，且非负性和DPI等保持成立，交叉熵$D(f||g)=\int f(x)\log\frac{f(x)}{g(x)}\mathrm{d}x$的定义和非负性如常。然而，线性变换下熵会发生变化：
\[
    h(AX+b)=h(AX)=h(X)+\log|A|,
\]
其中，$A$是与$X$的维度相同的可逆方阵。

## 2.最大熵
给定自然约束$\int f\mathrm{d}x=1$和其他约束$\int f(x)r_i(x)\mathrm{d}x=\alpha_i$（如各阶矩），使得熵最大的密度函数应为
\[
    f^*(x)=e^{\lambda_0+\sum_i \lambda_ir_i(x)},
\]
其中$\lambda$是待解参数，证明可参照交叉熵定义。
- 无其他约束的情况下，使得熵最大的密度函数应为均匀分布；
- 0期望和$\sigma^2$方差约束下，使得熵最大的密度函数应为正态分布；
- 固定期望为$\mu$，使得熵最大的密度函数应为参数为$\frac{1}{\mu}$的指数分布。

## 3.幂熵
微分熵的非负性并不成立，因此定义幂熵
\[
    N(X)=\frac{1}{2\pi e}e^{\frac{2}{k}h(X)}, X\in \mathcal{S}^k
\]
来保持非负性。正态分布$N(\mu,K)$的幂熵为$|K|^{1/k}$。对幂熵，不等式
\[
    N(X+Y)\geq N(X)+N(Y),
\]
成立，等号成立当且仅当它们的协方差矩阵的元素成比例。

## 4.习题
1. 对$n$个独立同分布的随机变量$X_i$，定义$S_j=\frac{1}{j}\sum_{i=1}^j X_i$为其样本均值。在离散和连续两种情况下求$\frac{1}{n}(S_1,\cdots,S_n)$。
2. 对$f$，给定与第2节相同的约束，求使得$D(f||g)$最小的分布$f$。
