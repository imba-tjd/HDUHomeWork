# CCF计算机职业资格认证考试

报名信息：`https://csp.ccf.org.cn/csp/signup/list_signup_personal.action`，第一次记得选团体报名，否则用不了优惠码。准考证要自己打印，临近时会给PDF下载。正式考试时只能用报名时选的语言，全程用那个。所以不能乱填。

三个我不会的语言标准为C++11，Java7，Python3.6，好评。C语言标准是C89，疯狂差评；直接也用C++11就是了，应该不会写出C有而C++没有的特性的代码，真用restrict和inline估计也没开优化没用。

模拟测试，题目应该都是过去的真题。测试网址：`http://118.190.20.162/UserEnter.do?USERKEY=<姓名>&NAME=<姓名>`，把`姓名`（不含尖括号）换成你的名字。乱填应该也行，只是记录就找不到了。那个C++我猜是C++99，反正没有任何优势就是了。

测试时能马上看到结果，但是正式考试的时候看不到。它只保存最后一次提交的代码，当时提交了是不知道对错的，只有考完后过一段时间出来的成绩。

可以带书，对树和图的算法不熟的同学可以考虑带一本数据结构。

我这里正式考试时的环境：Dev CPP开了`-std=c99`也跟没开一样；CodeBlocks是16年版的，且是英文；两者的intellisense都非常差；ctrl space是搜狗输入法。反正就是各种恶心。

# 做过的题

3月份的是测试题。9月份的是我自己正式考试的题，原题还没出来所以Remarks不全。其它测试题都不打算做了。

## [201903-1-小中大](./201903-1.c)

1. 取第一个数、中间的数、最后一个数。
2. 使用`%*d`可以丢弃读到的，不需要读入放到数组中；需要单独处理n为1和2的情况。
3. 比较一下第一个和最后一个数可得出原序列是从大到小还是从小到大的，从而进行处理。

### 经验

1. 中间的数的前者是`(n-1)/2`，不是`n/2-1`。
2. 中间的数对格式有要求，太好用%g来输出，因为它的精度参数是有效数字，无法直接控制小数。
3. 10^7相加除以二是不会溢出的，不过我的代码还是做了处理。
4. 判断浮点数是否为整数，我这样做其实没有考虑误差，虽然已经满分了。理论上需要相减后用fabs小于10-5来判断。

### 测试用例

```
3 -1 2 4
4 2 -1

4 -2 -1 3 4
4 1 -2

4 0 1 2 3
3 1.5 0

5 0 1 2 3 4
4 2 0
```

## [201903-2-二十四点](./201903-2.py)

考虑用python的`eval`，注意题目规定`/`是整除，并且乘法用的是字母x，所以要replace一下。这样非常非常简单，然而实际考试时必须全部用py才有机会。

如果用c写：

1. 因为要先算乘除，遇到任何数字都必须直接放到栈里。
2. 如果符号是乘除就直接和栈顶数字运算；如果是加减，只能把数字和符号都分别放到栈里，到最后再一起运算。
3. 基本上就是写一个计算器了，实在不想做。
4. 注意不能全部读到char[]里开始运算、只在最后`+'0'`，因为这只对加减有效，乘除就有变化了。
5. 必须用迭代，不能用递归，因为是整除，从左往右算跟反过来是不一样的，即使用double存也不行。

## [201903-3-损坏的RAID5](./201903-3.c)

1. 理解题目很重要；disks数组的大小花了一点时间才想清楚，和块的大小无关。
2. 想办法确定好位置后输出就是，如果没有就异或恢复。
3. 一个块的大小恰好是int大小，直接用char读取更方便，但之后要有一点转换。

### 测试用例

```
2 1 2
0 0001020304050607010111213141516172021222324252627
1 0001020304050607010111213141516172021222324252627
2
0
1

3 2 2
0 000102030405060710111213141516172021222324252627
1 A0A1A2A3A4A5A6A7B0B1B2B3B4B5B6B7C0C1C2C3C4C5C6C7
2
2
5
```

## [201909-1](./201909-1.c)

To be added.

### 测试用例

```
2 2
10 -3 -1
15 -4 0

3 3
73 -8 -6 -4
76 -5 -10 -8
80 -6 -15 0
```

## [201909-2](./201909-2.c)

* 那个表示“与”集合符号不要理解成异或了

### 测试用例

```
4
4 74 -7 -12 -5
5 73 -8 -6 59 -4
5 76 -5 -10 60 -2
5 80 -6 -15 59 0

5
4 10 0 9 0
4 10 -2 7 0
2 10 0
4 10 -3 5 0
4 10 -1 8 0
```

## [201909-3](./201909-3.c)

* 还是理解题目很重要，还是要分辨哪些是无用的信息
* 颜色仅需改变背景色，文字输出的是空格
* 所有输出都需要是纯文本，所以不能输出`\x20`，而是要用`\\x20`。而颜色的值比如说255，就要用`\\x32\\x35\\x35`
* block移动时，r要加上block的高(即q)，c要加block的宽(即p)，刚好感觉是反过来的
* 感觉就是干了很多脏活。然而对于我这个写过JSON Parser的人来说，这题并算不上多难；只是不敢写inline有点不爽

```
ESC：\x1B
[：\x5B
分号: \x3B
m: \x6D
空格: \x20
换行：\x1B\x5B\x30\x6D\x0A
改变背景：\x1B\x5B\x34\x38\x3B\x32\x3B R \x3B G \x3B B \x6D
```

### 测试用例

```
1 1
1 1
#010203

2 2
1 2
#111111
#0
#000000
#111
```

## 201909-5

看到第一个测试点的M仅为2，考虑用一个普通的dijskra。然而我没复习，临时写了结果失败了。

### 测试用例

```
5 2 2
1 5
1 2 4
1 3 5
1 4 3
4 5 1
```