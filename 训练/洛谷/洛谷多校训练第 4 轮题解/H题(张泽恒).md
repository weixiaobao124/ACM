<hr>

#### <center>H题 God J and Ham Sausage </center>

**题意：**

给定一个横置的火腿肠状的物体，中间为圆柱，两头为半球，圆柱部分长为 $H$ , 半径为 $R$ 。

给出一个切割间距 $D\leq 2 * R + H$ ， 从最左边开始间隔 $D$ 切割一次，输出从左到右每个被切割下来部分的体积。

**题解：**

由于可能出现若干情况，利用前缀做差可以比较容易实现，且分类讨论较为容易。

此时应该解决的是半球没有被完全切割时的体积。

设在距离球顶 $h$ 进行切割，其切下来的带球顶的那部分体积为$CUT(h)$

通过简单定积分即可计算：$CUT(h) = \int_{0}^{h}(R^2-(R-h)^2*\pi)dh$

化简后可以得到:
$$
CUT(h)= \pi*(R*h^2-\frac{h^3}{3})
$$
我们通过封装一个函数：传入切割位置，返回从最左端到该切割距离的体积，压入$vector$

最后如果没有切完记得把整个体积也要压入$vector$

最后遍历做差即可得到每段的答案。

**时间复杂度：** $O(2*R + H)$

**AC代码：**

```c++
#include<bits/stdc++.h>
using namespace std;   
typedef pair<int, int> pii;
double h, r, d;
double pi = acos(-1);
vector<double> ans;
vector<double> ansd;
double getv(double H)
{
	if(H <= r) return pi * (r * H * H - 1.0 / 3.0 * H * H * H);
	double V1 = 2.0 / 3 * r * r * r * pi;
	if(H <= r + h)
	{
		double V2 = (H - r) * pi * r * r;
		return V1 + V2;
	}
	double tot = V1 * 2.0 + pi * r * r * h;
	H = 2.0 * r + h - H;
	double ri =pi * (r * H * H - 1.0 / 3.0 * H * H * H);
	return tot - ri;
}
int main()
{
	scanf("%lf%lf%lf", &h, &r, &d);
	for(double D = d; D <= h + 2.0 * r; D += d)
	{
		double V = getv(D);
		ans.push_back(V);
		ansd.push_back(D);
	}
	if(ansd.back() < 2.0 * r + h) ans.push_back(getv(2.0 * r + h));
	printf("%0.10lf\n", ans.front());
	for(int i = 1; i < ans.size(); ++i)
		printf("%0.10lf\n", ans[i] - ans[i - 1]);
    return 0;
}
```



<hr>
