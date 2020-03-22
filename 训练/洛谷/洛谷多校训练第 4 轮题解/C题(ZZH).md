<hr>

#### <center> [C题 God J and Chests](https://www.luogu.com.cn/problemnew/show/T125993)  </center>

**题意：** 给 $n$ 个集合，每个集合 $m$ 个数从集合中选一个数，使得选出的数方差最小。

$1\leq n, m\leq 1000$

$1\leq a_{i,j}\leq 10^6$

**题解：**

自己假设一个平均数设为 $A$ ,因为方差描述的是数据的集中程度，每个集合一定会选取和 $A$ 最接近的，暂且不考虑在多个等距离选择的情况。选法是固定的，此时真正的平均数为 $S$ 。

带入方差做差并只考虑分子：

$\sum_{i=1}^{n}[(A-x_i)^2-(S-x_i)^2]$

$=\sum_{i=1}^{n}[A^2-2Ax_i+x_i^2-S^2+2Sx_i^2-x_i^2]$

$=\sum_{i=1}^{n}[A^2-S^2+2x_i(S-A)]$

$=nA^2-nS^2+2nS(S-A)$

$=nA^2-2nAS+nS^2$

$=n(A-S)^2$

可以发现自己用 $A$ 计算的方差一定大于等于真实值。

此时考虑存在$A$ 可能存在多个等距离选项的情况。

假设有 $n$ 个多选的集合(矛盾集合)，剩下还有 $m$ 个集合不会出现这样的情况（和平集合）。

设 $n$ 个矛盾集合依次的偏移量为 $x_i$ ，所以选择的数为 $A+x_i$ 或 $A-x_i$,

设其选择的数为 $A+flag_ix_i(flag_i=1或-1)$

设 $m$ 个和平集合依次选择的数为 $y_i$

设 $D=\sum_{i = 1}^{n}x_i$

设 $F=\sum_{i=1}^{n}x_i^2$

设 $B=\sum_{i=1}^{m}y_i$

设 $C=\sum_{i=1}^{m}y_i^2$

设 $t=\sum_{i=1}^{n}flag_ix_i(flag_i=-1或1)$

$均值EX=\frac{nA+B+t}{n+m}$

此时方差$DX=EX^2-(EX)^2$

$(EX)^2=\frac{n^2A^2+B^2+t^2+2nAB+2nAt+2Bt}{(n+m)^2}$

$EX^2=\frac{\sum_{i=1}^{n}(A+flag_ix_i)^2+\sum_{i=1}^{m}y_i^2}{(n+m)}$

$=\frac{\sum_{i=1}^{n}(A^2+2flag_ix_iA+x_i^2)+C}{(n+m)}$

$=\frac{nA^2+2\sum_{i=1}^{n}flag_ix_iA+\sum_{i=1}^{n}x_i^2)+C}{(n+m)}$

$=\frac{nA^2+2At+F+C}{(n+m)}$

$=\frac{A^2(n^2+mn)+2At(n+m)+F(n+m)+C(n+m)}{(n+m)^2}$

$EX^2-(EX)^2=\frac{A^2mn+2mAt-B^2-t^2-2nAB-2Bt+F(n+m)+C(n+m)}{(n+m)^2}$

是关于 $t$ 的二次函数，且 $t$ 的系数小于0,$t$ 的定义域为 $t\in[-D,D]$ 极小值点一定是端点的其中一个。

所以当出现矛盾时候可以全部选取 $A$ 的同一侧来消除矛盾。 

有了理论基础，现在可以提出算法的流程。

总的来说我们可以先枚举 $A$ 得到具体选法后通过调整 算出真正的平均值$\overline S$ 

如果对每个集合内部排序一次，可以发现 $A$ 一定落在两头后者两个元素之间：

$A < \frac{a_1+a_2}{2}$ 选择$a_1$

$A<\frac{a_2+a3}{2}$, 选择$a_2$

$···$

于是我们得到 $n * (m - 1)$ 个分界点，遍历分解点，不断调整即可快速算出当前的答案

现在给出答案的计算方法：

$D(X)n^2=\frac{\sum_{i=1}^{n}(x_i-\overline S)^2}{n}*n^2$

$=n\sum_{i=1}^{n}(x_i-\overline S)^2$

$=n\sum_{i=1}^{n}(x_i^2-2x_i\overline S+\overline S^2)$

$=\sum_{i=1}^{n}(nx_i^2-2x_in\overline S+n\overline S^2)$

$=\sum_{i=1}^{n}(nx_i^2-2x_iSUM)+n^2\overline S^2$

$=\sum_{i=1}^{n}(nx_i^2-2x_iSUM)+SUM^2$


我们遍历分解点的时候只会一个集合中的数，维护等式即可。

**时间复杂度：** $O(N*M)$

**期望补充：**

$E(X)=\int_{-\infty}^{+\infty}xf(x)dx$

$E(C)=C$

$E(aX)=aE(X)$

$E(X)=E(Y)$

$E(XY)=E(X)E(Y)(独立)$

**方差补充：**

$D(X)=\frac{\sum_{k=1}^{n}(X_k-\overline{X})^2}{n}$

$D(X)=E(X^2)-(E(X))^2$

$D(C)=0$

$D(aX+b)=a^2D(X)$



<hr>
