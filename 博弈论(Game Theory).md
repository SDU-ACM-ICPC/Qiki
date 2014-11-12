零、「以牙还牙，以眼还眼」
===

见 [Codeforces 197A Plate Game](http://codeforces.com/problemset/problem/197/A)。

（经典博弈，我们选修课老师在上课时提到过此题）

分析：一开始你放一个盘子在桌子中心，然后对手怎么放，你就关于桌子中心对称地放，只要一开始你能放一个盘子在中心，那么最后你肯定能赢。

此乃「以牙还牙，以眼还眼」。

一、基础知识
===

一般规则：
1. i=0为必胜/必败态（视题目而定）；
2. 若**某个**i-(某个数)为必败态的话，i为必胜态 / 若i为必败态，则i+(某个数)为必胜态；
3. 若**所有**i-(某个数)为必胜态的话，i为必败态 / 若i为必胜态，则i+(某个数)可能为必败态，**但是若i+(某个数)只有一个后继，则其必为必败态**。

技巧：

如果难以看出必胜/必败态，不妨从i=0开始画一棵必胜/必败态的转移树来辅助分析。

一些题目使用记忆化搜索就可以解决。

\>> 有最大取子限制的取石子游戏：[POJ 2068 Nim](http://poj.org/problem?id=2068)

\>> 需要一些技巧性讨论的取石子游戏：[POJ 1740 A New Stone Game](http://poj.org/problem?id=1740)

\>> 欧几里得博弈：[POJ 2348 Euclid's Game](http://poj.org/problem?id=2348) (好题)

\>> 弗格森 (Ferguson) 游戏（大白书 P134）

\>> Chomp! 游戏（大白书 P134）

\>> 约束游戏（大白书 P135）

\>> 日历博弈：[POJ 1082 Calendar Game](http://poj.org/problem?id=1082)

**巴什 (Bash) 博奕**
[HDU 1846 Brave Game](http://acm.hdu.edu.cn/showproblem.php?pid=1846)

有 n 个石子，两个人轮流取 1~m 个石子。最后取光者得胜。

结论：当 `$m+1\mid n$` 时后手胜，否则先手胜。

分析：以牙还牙。当 `$m+1\mid n$` 时，先手取 x 个，那么后手取 m+1-x 个，最后一定是后手取完；当 `$m+1\nmid n$` 时，设 `$n=k(m+1)+r$`，一开始先手取 r 个，那么变成前面 `$m+1\mid n$` 的局面，最后先手取完。

\>> [威佐夫 (Wythoff) 博奕与 Beatty 定理](https://www.google.com.hk/?gws_rd=ssl#newwindow=1&safe=strict&q=%E5%A8%81%E4%BD%90%E5%A4%AB%E5%8D%9A%E5%BC%88)

\>> [斐波那契 (Fibonacci) 博弈与 Zeckendorf 定理](https://www.google.com.hk/?gws_rd=ssl#newwindow=1&q=%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E5%8D%9A%E5%BC%88)

二、Nim
===

该游戏起源于中国，经当年到美洲打工的华人流传出去。其英文名「NIM」是由广东话「拈」（取物之意）音译而来。

1902 年，L.Bouton 脑洞大开，提出了如下定理，从而彻底地解决了 Nim 问题：
```
异或和值为零则后手胜，否则先手胜。
```
而这个定理两句话就能解释明白：
1. 当异或和值为零时，无论你怎么取，异或和值一定变为非零，对手以牙还牙，使异或和值变为零，如此循环，直至对手取完。故此时先手必败（处于必败态）。
2. 当异或和值非零时，总能在某一堆中选取若干个石头，使得异或和值为零（可以自己找几个例子算算），则必胜态总能转移到必败态。

一些问题可以转化为 Nim 问题，常用的思路是两个两个分成一组进行思考，注意利用“以牙还牙，以眼还眼”这一技巧。

例题：

[UVa 12499 I am Dumb 3](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3943)

[HDU 4315 Climbing the Hill](http://acm.hdu.edu.cn/showproblem.php?pid=4315)

下面阶梯博弈的变形2也可以用分组的思想做。

**阶梯博弈(Staircase Nim)**

博弈在一列阶梯上进行，每个阶梯上没有石子或放着若干个石子。两个人进行阶梯博弈，每一步则是将阶梯（不是第一层的阶梯）上的若干个石子移到（某些指定的）低阶的阶梯去，最后没有石子可以移动的人输。
![](http://hi.csdn.net/attachment/201108/16/0_1313488616cRqz.gif)

阶梯博弈可以利用游戏的限制转化成Nim解决：由于某个阶梯上的石子移动到不能移动时所需的步数要么是奇数，要么是偶数。对于偶数步，对方用哪些石子走一步你也用哪些石子走一步，从而不影响游戏结果。所以**只对奇数阶的石子求Nim就行**。

例题：[HDU 3389 Game](http://acm.hdu.edu.cn/showproblem.php?pid=3389)

有一些看似与阶梯博弈无关的题，揭开它的面纱后，实际上就是阶梯博弈：
1. 博弈在一个N个格子的方块上进行，每个方块上至多有一个石子，每次可以选择一个石子向右移动到最近的空位，不能移动石子的人输，如下图所示：

![](http://endless.qiniudn.com/blognim.jpg)

设函数F[i]表示第i个石子移动到不能移动所需要的步数。

然后我们来观察某一次移动，假设图中的2跳到了4的后面，我们可以来观察一下所有石子的F函数有什么变化：

![](http://user-image.logdown.io/user/7190/blog/7143/post/196410/ovQkpA6uTiq2DGCqwA74_1453255304857576105(2).jpg)

首先对于1及其之前的石子，它们的F函数显然不变。

同样，对于移动后的2之后的石子，它们的F函数也不变。

那么变化的就是2,3,4这三个石子的F值，变化了多少？每一个减少了1

那么，如果我们把这个函数F[]看做台阶的阶数的话，乃是不是觉得有一点熟悉的感觉了~

没错！就是阶梯博弈

·2. [POJ 1704 Georgia and Bob](http://poj.org/problem?id=1704)

也是一行格子和一些石子，每次可以把一个石子往左移动若干步，但是不能越过石子，谁最后不能移动谁输。

如果将没有石子的格子看做阶梯上的石子，我们会发现移动一个石子实际上是将这个石子左边的**格子移到右边去**，也就是把高度为i的阶梯上的石子移到下一个阶梯上去（最右边的空白为第一阶），所以还是讨论奇偶性~

（这题也可以用两个两个一组的思路来思考）

三、Grundy值(SG函数)
===

我们把 Nim 游戏加点限制：每次你只能取a1,a2,a3,...an个石头。

如何转化成 Nim 问题呢？

原来的 Nim 是 a->a' 的转化，现在我们改为 SG(a)->SG(a') 的转化。

见 [Wiki](http://en.wikipedia.org/wiki/Mex_(mathematics)) 中给出的例子。

Sprague-Grundy 定理(SG 定理)：**游戏和**的Grundy值等于各游戏Grundy值的异或和。

很多博弈游戏都含有**游戏和**这一要素，故可用Grundy值很方便的解决。

**剪纸博弈**

[POJ 2311 Cutting Game](http://poj.org/problem?id=2311)（代码见下）

<!--more-->

```c++
/*188ms,848KB*/

#define Fpos(a, n, x) (find(a, a + (n), x) - a)
const int mx = 205;

int dp[mx][mx];

int sg(int w, int h)
{
	if (~dp[w][h]) return dp[w][h];
	bool vis[mx] = {0};
	int i;
	Forr(i, 2, w - 1) vis[sg(i, h) ^ sg(w - i, h)] = true; /// 分解出子游戏
	   Forr(i, 2, h - 1) vis[sg(w, i) ^ sg(w, h - i)] = true;
	return dp[w][h] = Fpos(vis, mx, false);
}

int main()
{
	mem(dp, -1);
	int w, h;
	while (~SII(w, h)) puts(sg(w, h) ? "WIN" : "LOSE");
	return 0;
}
```

四、其他
===

**翻硬币游戏 (Coin Turning Game)**
1. 一维的翻硬币游戏，每次可以翻1个或两个。通过单独考虑每个可以翻的硬币发现，Coin Turning Game的SG值和Nim等价，因此两个模型等价。需要注意的是，许多翻硬币游戏根据题目的要求，一般编号从0开始。
2. 一维的翻硬币游戏，每次可以翻1个或两个，限定了翻第二枚硬币的范围，那么就和Subtraction Game等价了。
3. 一维的翻硬币游戏，每次可以翻1个、2个或3个，这个游戏叫做Mock Turtles，有一个神奇的规律，是Odious Number序列。
4. 高维的翻硬币游戏，需要用到Nim积和Tartan定理。
翻硬币模型的变化更多，很多模型都有一些奇妙的规律，需要打表才能发现。
  
**删边游戏(Green Hackenbush)**
1. 树的删边游戏：Colon原理证明这种模型和Nim依然是等价的，多个叉的SG值异或就是对应根节点的SG值。
2. 无向图删边游戏：利用Fursion定理收缩圈，然后就转换成树的删边游戏了，不过这个定理还不会证。 
