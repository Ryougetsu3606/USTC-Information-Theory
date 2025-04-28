# 信息论笔记第1讲
本笔记的内容基于25春张文逸老师《信息论》的授课内容，个人感觉难度较大，包含了许多工科生少用到的分析构造技巧。虽然对个人未来的AI科研（可能）没什么用，但不失为一个活跃思维的好机会，因此做下这份笔记供参考。本讲略去了无穷马尔科夫链的内容，之后可能会补上。
## 1.几种熵

### 信息熵
定义为
$$
    H(X)=-\sum_{x\in X(\Omega)}P_X(x)\log P_X(x),
$$
其中$X$是随机变量，其分布函数为$P_X(x)$，定义在$X(\Omega)$上。

- 参数为$p$的0-1分布，其信息熵记为
$$
    h_2(p)=-p\log p-(1-p)\log(1-p),
$$
对其求导得到
$$
    h_2'(p)=\log\frac{1-p}{p},
$$
可作为一个参考结果。其图像类似于开口向下的抛物线，在$p=0,1$时取值为0，在$p=\frac{1}{2}$时取到最大值1。
- 定义在$\{1,2,\cdots,M\}$上的均匀分布$X$，满足$H(X)=\log M$。
- 参数为$\epsilon$的几何分布$Y$，有$P_Y(y)=\epsilon(1-\epsilon)^{y-1}$，其信息熵为$H(Y)=\frac{h_2(\epsilon)}{\epsilon}$。

### 联合信息熵
记为
$$
    H(X,Y)=-\sum_{(x,y)\in X(\Omega)\times Y(\Omega)}P_{X,Y}(x,y)\log P_{X,Y}(x,y).
$$
要计算的项数等于$|X(\Omega)||Y(\Omega)|$，比较麻烦。

### 条件信息熵
对给定的$X$，$Y$的**条件信息熵**为
$$
    \begin{aligned}
        H(Y|X)&=\sum_{x\in X(\Omega)}P_X(x)H(Y|X=x)\\
        &=-\sum_{(x,y)\in X(\Omega)\times Y(\Omega)}P_{X,Y}(x,y)\log P_{Y|X}(y|x).
    \end{aligned}
$$
还是要算$|X(\Omega)||Y(\Omega)|$项。

类似地，我们有联合条件信息熵$H(Y_1,\cdots,Y_n|X_1,\cdots,X_m)$，计算方式同上。实际在做题的时候，诸如$x\in X(\Omega)$和$P_X(x)$中的$X$都可以省略（甚至采用小写字母$p$），以简化式子。

### 交叉熵/KL距离/I散度
交叉熵的定义在机器学习里已经见过了，其作用是：度量两个概率分布的相似程度。因此可用作神经网络的损失函数，以使拟合出的分布逼近于训练集的分布。

分布$P(x)$和$Q(x)$的交叉熵记作
$$
    D(P\|Q)=\sum_{x}P(x)\log\frac{P(x)}{Q(x)}.
$$
它并不对称，也不满足三角不等式（i.e. $D(P\|Q)+D(Q\|R)\geq D(P\|R)$）。

### 互信息
互信息的定义来自交叉熵
$$
    \begin{aligned}
        I(X;Y)&=D(P_{X,Y}\|P_XP_Y)\\
        &=\sum_{x,y}P(x,y)\log\frac{P(x,y)}{P(x)P(y)}.
    \end{aligned}
$$
其中$\log$的部分叫做互信息密度$i(x;y)$。

### 条件互信息
$$
    \begin{aligned}
        I(X;Y|Z)=\sum_{x,y,z}p(x,y,z)\log\frac{p(x,y|z)}{p(x|z)p(y|z)}.
    \end{aligned}
$$

## 2.概率论拾遗
写到一半想起来张老师讲义里重新定义了马尔科夫链，这里说明一下。
- **定义**：对随机变量$X,Y,Z$，我们称其为符合$X\leftrightarrow Y\leftrightarrow Z$的马尔科夫链，当
$$
    P(x,z|y)=P(x|y)P(z|y),\,\forall (x,y,z)\in X(\Omega)\times Y(\Omega)\times Z(\Omega).
$$
即$X$和$Z$关于$Y$条件独立。

展开上式得到
$$
    \begin{aligned}
        \frac{P(x,y,z)}{P(y)}&=\frac{P(x,y)}{P(y)}\frac{P(y,z)}{P(y)}\Leftrightarrow \\
        P(x,y,z)P(y)&=P(x,y)P(y,z)\Leftrightarrow \\
        \frac{P(x,y,z)}{P(x,y)}&=\frac{P(y,z)}{P(y)}\Leftrightarrow \\
        P(z|x,y)&=P(z|y),
    \end{aligned}
$$
与我们熟知的马尔科夫链定义等价。

## 3.链式法则
接下来几节我们介绍熵和信息量的一些性质。

### 熵的链式法则
- 基本形式
$$
    H(X,Y)=H(X)+H(Y|X)=H(Y)+H(X|Y).
$$
- 条件形式
$$
    H(X,Y|Z)=H(X|Z)+H(Y|X,Z)=H(Y|Z)+H(X|Y,Z).
$$
- 整体形式
$$
    H(X_1,\cdots,X_n)=\sum_{i=1}^n H(X_i|X_{i-1},\cdots,X_1),
$$
其中$X_0$看作常数。

以上性质的证明均可由信息熵的定义，以及归纳法直接得出。

### 互信息的基本性质
- 对称性
$$
    I(X;Y)=I(Y;X).
$$
- 与熵的关系
$$
    I(X;X)=H(X).
$$
- 分解
$$
    \begin{aligned}
        I(X;Y)&=H(X)-H(X|Y)\\
        &=H(Y)-H(Y|X)\\
        &=H(X)+H(Y)-(X,Y).
    \end{aligned}
$$
- 条件分解
$$
    \begin{aligned}
        I(X;Y|Z)&=H(X|Z)-H(X|Y,Z)\\
        &=H(Y|Z)-H(Y|X,Z)\\
        &=H(X|Z)+H(Y|Z)-(X,Y|Z).
    \end{aligned}
$$
由互信息的定义可以直接证明以上性质，建议自己推一推练练手。

### 互信息的链式法则
对随机变量$X_1,\cdots,X_n,Y$，有
$$
    I(X_1,\cdots,X_n;Y)=\sum_{i=1}^n I(X_i;Y|X_{i-1}\cdots,X_1),
$$
把所有互信息展开为熵即可。

- 一个推广：Csiszár恒等式
$$
    \sum_{i=1}^n I(X_{i+1},\cdots,X_n;Y_i|Y{i-1}\cdots,Y_1)=    \sum_{i=1}^n I(Y_{i-1},\cdots,Y_1;X_i|X{i+1}\cdots,X_n)
$$
证明参见[这里](https://math.stackexchange.com/questions/4295903/csiszar-sum-identity-proof)

## 4.非负性
### 交叉熵的非负性
交叉熵$D(P\|Q)\geq 0$恒成立，等号成立当且仅当对于所有$x$，有$P(x)=Q(x)$。

按定义展开即可，注意到用不等式$\ln x\leq x-1$放缩可以消去对数，转为线性问题，且各不等式可以同时取等。

- **推论1** *熵的上下界*：$0\leq H(X)\leq \log|X(\Omega)|$，左边的等号在$X$为常数时取到，右边的等号在$X$为均匀分布时取到。
- **证明**：下界的证明略。上界的证明可通过引入参考分布$P_{X,u}(x)=\frac{1}{|X(\Omega)|}$，得到
$$
    \begin{aligned}
        D(P_X\|P_{X,u})&=\sum_x P_X(x)\log\frac{P_X(x)}{P_{X,u}(x)}\\
        &=\sum_x P_X(x)\log(|X(\Omega)|P_X(x))\\
        &=-H(X)+\log|X(\Omega)|\geq 0.
    \end{aligned}
$$
考虑取等条件即可。

- **推论2** *给定期望的熵的上下界*：对于$X(\Omega)=\mathbb{N}^*$，且期望为给定的$\mathbb{E}X=A>1$，其熵的上界在$P_X(x)=\frac{1}{A}(1-\frac1A)^{x-1}$，即服从参数为$\frac{1}{A}$的几何分布时取到。证明类似于推论1，同样取$X(\Omega)$上的参考分布。

### 互信息的非负性
- 互信息$I(X;Y)\geq 0$恒成立，等号成立当且仅当$X$与$Y$独立。对于联合分布的情形应当是类似的，即：整体独立。证明可由$I(X;Y)=D(P_{X,Y}\|P_XP_Y)$得到。注意取等条件。


- 互信息$I(X;Y|Z)\geq 0$恒成立，等号成立当且仅当在给定的$Z$的条件下，$X$与$Y$独立。即有马尔科夫链$X\leftrightarrow Z\leftrightarrow Y$。证明由$I(X;Y|Z)=\sum_z p(z)\sum_{(x,y)} D(P_{X,Y|Z}\|P_{X|Z}P_{Y|Z})$得到。

### 数据处理不等式（DPI）
若有马尔科夫链$X\leftrightarrow Y\leftrightarrow Z$，则$I(X;Y)\neq I(X;Z)$，等号成立当且仅当$X\leftrightarrow Z\leftrightarrow Y$。证明结合链式法则与互信息的非负性即可。这一不等式可以理解为：信息经过的处理次数越多，与原信息之间的误差就越大。

特别地，函数作用$f(\cdot)$可看作一种信息处理，代换上式中的$Z$（因为$f(Y)$显然可以只与$Y$有关），即$I(X;Y)\neq I(X;f(Y))$。等号成立当且仅当$X\leftrightarrow f(Y)\leftrightarrow Y$，*即仅由$f(Y)$可以恢复出$Y$，亦即$f(\cdot)$是双射。（此句存疑，敬请修正）*

## 5.条件损失
这一性质可以概括为：加条件一定会损失信息（熵）。
### 条件降熵
对随机变量$X,Y$和$Z$，总有
$$
    \begin{aligned}
        H(X)&\geq H(X|Y),\\
        H(X|Z)&\geq H(X|Y,Z).
    \end{aligned}
$$
拆成互信息即证。此外有
$$
    H(X_1,\cdots,X_n)\leq\sum_{i=1}^n H(X_i),
$$
等号成立当且仅当$X_1,\cdots,X_n$相互独立。

- **推论**：给定$X$，$H(Y|X)=0$当且仅当$Y=f(X)$；$H(f(X))\leq H(X)$总是成立，等号成立当且仅当$f$是双射。

前者由条件熵的定义可证，后者考虑$H(X,f(X))$的两种链式分解方式。

注意，对互信息，条件并不降熵。

### 互信息的凹凸性
给定分布$Y|X$，$I(X;Y)$关于$X$是凹的；给定分布$X$，$I(X;Y)$关于$Y|X$是凸的。证明的思路是，引入一个参数为凸参数$\lambda$的二项分布$Q$，类似于DPI的过程进行证明。这个结论好像不是特别重要。

## 6.Fano不等式
考虑这样的场景：对于随机变量$X,Y$，我们仅知道其联合分布$P_{X,Y}$，且想通过观察$Y$来判断$X$的值。这个**观察**的过程可以得到一个随机变量$\hat{X}$，且它仅取决于$Y$，即有马尔科夫链$X\leftrightarrow Y\leftrightarrow \hat{X}$。又记观察错误率为$P_e=P(\hat X\neq X)$。

现在我们想**最小化**$P_e$，那么有定理如下：对一个观察到的$y$，
$$
    \hat{X}=\argmax_x P(x|y).
$$
- **证明**：引入条件和联合分布，并注意马尔可夫链，得到
$$
    \begin{aligned}
        P_e&=\sum_x p(x)P(\hat{X}\neq x|X=x)\\
        &=\sum_x p(x)\sum_y P(\hat{X}\neq x,Y=y|X=x)\\
        &=\sum_x p(x)\sum_y P(\hat{X}\neq x|Y=y, X=x)p(y|x)\\
        &=\sum_x p(x)\sum_y [1-P(\hat{X}= x|Y=y, X=x)]p(y|x)\\
        &=\sum_x p(x)\sum_y (1-p(\hat{x}|y))p(y|x)\\
        &=\sum_{(x,y)}p(x)p(y|x)(1-p(\hat{x}|y))\\
        &=1-\sum_{(x,y)}p(x,y)p(\hat{x}|y)\\
        &=1-\sum_y p(y)\sum_x p(x|y)p(\hat{x}|y),
    \end{aligned}
$$
即，对每个$y$，**最大化**内部的求和$\sum_x p(x|y)p(\hat{x}|y)$。因此只需选择$\hat{x}$分配到最大的$p(x|y)$即可。

现在，我们有Fano不等式
$$
    H(X|Y)\leq H(X|\hat{X})\leq h_2(P_e)+P_e\log(|X(\Omega)|-1).
$$
由DPI可知第一个不等号成立。对第二个不等号，引入参考变量
$$
    \begin{aligned}
        E=\begin{cases}
            1,\,\text{if }\hat{X}\neq X,\\
            0,\,\text{if }\hat{X}= X,\\
        \end{cases}
    \end{aligned}
$$
由链式法则和条件降熵知
$$
    \begin{aligned}
        H(X|\hat{X})&=H(X|\hat{X},E)+H(E|\hat{X})-H(E|X,\hat{X})\\
        &=[P(0)H(X|\hat{X},0)+P(1)H(X|\hat{X},1)]\\&+H(E|\hat{X})-0\\
        &\leq [(1-P_e)\cdot 0+P_e\cdot \log(|X(\Omega)|-1)] + h_2(P_e),
    \end{aligned}
$$
最后一个等号的第一项是由于$E=0$时$X=\hat{X}$固定，第二项是考虑$H(X)$的上界（损失一种可能性后），第三项是去条件放大熵。等号成立的条件容易探明：①是$E$与$\hat{X}$独立，②是$X$在给定$\hat{X}$下，不等于$\hat{X}$时是均匀分布，即

$$
    P_{X|\hat{X}}(x|\hat{x})=\begin{cases}
        1-P_e,x=\hat x,\\
        \frac{P_e}{|X(\Omega)|-1},x\neq\hat x.
    \end{cases}
$$

Fano不等式是信息论处理条件熵**上界**的标准工具。

## 7.习题
留几道很有意思的习题作为参考：
1. 考虑$\mathbb{N}^*$上的随机变量$X$，其概率分布关于$x$单调非增。现在给定一个数字$n\in\mathbb{N}^*$让我们猜，求证：平均猜测次数不小于$\mathrm{e}^{H(X)-1}$。（单位：奈特）（提示：期望与熵的关系？）
2. 考虑$\mathbb{N}^*$，对每个正整数，以$p$的概率选取之，以独立地生成两个随机子集$A$和$B$。计算$H(A)$和$H(A\cup B)$。
3. 设$Z$是$\mathbb{N}^*$上的随机变量，$X$是以$2^{-Z}$为参数的几何分布，求证：若$Z$的期望无穷大，则$X$的熵无穷大。又，令$Y$以$\epsilon$的概率取值为$X$，否则为0，而$\hat{Y}$恒为0。证明：若$X$的熵无穷大，则$H(\hat{Y}|Y)$不随$\epsilon$的下降收敛到0。这给出法诺不等式在无穷集合上的应用。