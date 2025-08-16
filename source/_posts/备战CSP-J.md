---
title: 备战 CSP-J
date: 2025-08-13 08:40:30
tags: ['OI', 'CSP-J']
categories: ['学习', '努力','备考']
katex: true
author: ED_Builder
---

很难很累，但还是必须去做

<!-- more -->

# 任务清单
## 一轮真题
[{% checkbox checked:true 2019 CSP-J1%}](https://ti.luogu.com.cn/problemset/1030)  
[{% checkbox checked:true 2020 CSP-J1%}](https://ti.luogu.com.cn/problemset/1034)  
[{% checkbox checked:true 2021 CSP-J1%}](https://ti.luogu.com.cn/problemset/1036)  
[{% checkbox checked:true 2022 CSP-J1%}](https://ti.luogu.com.cn/problemset/1039)  
[{% checkbox checked:true 2023 CSP-J1%}](https://ti.luogu.com.cn/problemset/1041)  
[{% checkbox checked:true 2024 CSP-J1%}](https://ti.luogu.com.cn/problemset/1043)  
## 二轮真题
{% checkbox checked:true symbol:minus color:yellow 2019 CSP-J2%}
{% checkbox checked:true symbol:minus color:yellow 2020 CSP-J2%}
{% checkbox checked:true symbol:minus color:yellow 2021 CSP-J2%}
{% checkbox checked:true symbol:minus color:yellow 2022 CSP-J2%}
{% checkbox checked:true symbol:minus color:yellow 2023 CSP-J2%}
{% checkbox checked:true symbol:minus color:yellow 2024 CSP-J2%}
# 题目选讲
## 2019 CSP-J1
### 1. 常识积累
- T1：中国的国家顶级域名是 `.cn`
- T3：一个 32 位整形变量占用 {% mark 4 个字节 %}
- T5：折半查找（二分）时，找到目标最多需要 {% mark log(n) 次 %}
- T6：链表在 {% mark 插入和删除 %} 中表现优异，在 {% mark 查询 %} 中表现较差，也不必事先估计存储空间
- T9：100 以内的最大质数是 {% mark 97 %}
- T15：计算机科学领域的最高奖是 {% mark 图灵奖 %}
### 2. 数论计算
#### T7（整数划分）
把 $8$ 个同样的球放在 $5$ 个同样的袋子里，允许有的袋子空着不放，问共有多少种不同的分法？
提示：如果 $8$ 个球都放在一个袋子里，无论是哪个袋子，都只算同一种分法。
{% checkbox A. 22%}
{% checkbox B. 24%}
{% checkbox checked:true C. 18%}
{% checkbox D. 20%}
解析：直接暴力枚举，然后去重就没了
#### T12（鸽巢原理）
一副纸牌除掉大小王有 52 张牌，四种花色，每种花色 13 张。  
假设从这 52 张牌中随机抽取 13 张牌，则至少（）张牌的花色一致。
{% checkbox checked:true A. 4%}
{% checkbox B. 2%}
{% checkbox C. 3%}
{% checkbox D. 5%}
解析：设在 12 张牌中，每种花色 3 张（最坏情况），那么再多一张一定会有 4 张牌的花色一致
#### T13（乘法原理）
—些数字可以颠倒过来看，例如 0,1,8 颠倒过来还是本身，6 颠倒过来是 9，9 颠倒过来看还是 6，其他数字颠倒过来都不构成数字。
类似的，一些多位数也可以颠倒过来看，比如 106 颠倒过来是 901。假设某个城市的车牌只由 5 位数字组成，每一位都可以取 0 到 9。
请问这个城市最多有多少个车牌倒过来恰好还是原来的车牌？（）
{% checkbox A. 60%}
{% checkbox B. 125%}
{% checkbox checked:true C. 75%}
{% checkbox D. 100%}
解析：在 0,1,8,6,9 中选择，然后判断回文数（因为顺序颠倒之后还要求是原来的车牌），那么我们只需要考虑第 1，2，3 位就好了  
但是颠倒之后 6 和 9 会互换，所以第 3 位不能是 6 和 9  
所以第一、二位有 5 种选择，第三位有 3 种选择，根据乘法原理可得最终方案数为 $5\times5\times3=75$
## 2024 CSP-J2 T2（大模拟）
题号：[Luogu P11228](https://luogu.com.cn/problem/P11228)
### 题意
给出 $n$ 行 $m$ 列的地图，机器人初始位于 $(i,j)$，朝向是 $d$，每次根据当前朝向朝对应方向步进，如果下一步越界或不是空地，那就向右转。问进行 $k$ 次操作之后，机器人走过的格子数量（包括起始位置）
### 解析
依题意模拟，用一个集合记录走过的格子，当机器人操作数等于 $k$ 时，输出机器人走过的格子数量，然后结束跑下一组数据即可。
### 代码
```cpp
#include<bits/stdc++.h>
using namespace std;
bool vis[1005][1005];//走过的格子标记
char mp[1005][1005];//地图
int dx[]={0,1,0,-1};
int dy[]={1,0,-1,0};//位移
void init()
{
    memset(vis,false,sizeof(vis));//重置为都没被访问
    return;
}
void solve()
{
    int n,m,k,x,y,d,ans=0;
    cin>>n>>m>>k>>x>>y>>d;
    vis[x][y]=true;//起始位置已被访问
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            cin>>mp[i][j];
    for(int i=1;i<=k;i++)
    {
        int nxt_x=x+dx[d],nxt_y=y+dy[d];//计算下一个点
        if(nxt_x<1||nxt_x>n||nxt_y<1||nxt_y>m||mp[nxt_x][nxt_y]=='x')
        {
            d=(d+1)%4;//如果越界或撞墙，就转
            continue;
        }
        vis[nxt_x][nxt_y]=true;//下一个点已被访问
        x=nxt_x,y=nxt_y;//更新到下一个点
    }
    for(int i=1;i<=n;i++)//统计走过的格子数量
        for(int j=1;j<=m;j++)
            if(vis[i][j])  ans++;
    cout<<ans<<endl;
    return;
}
int main()
{
    int T;
    cin>>T;
    while(T--)
    {
        init();
        solve();
    }
    return 0;
}
```
## 2021 CSP-J2 T3（字符串+模拟）
### 题意
给你 $n$ 台机器，分为服务器和客户端。  
{% mark 难点%} 然后判断服务器和客户端的 IP（IPv4+端口）是否合法  
然后服务器的 IP 必须保持唯一  
然后客户端要和对应的 IP 建立连接，按题意输出就好了
### 解析
对于验证 IP 的正确性问题，我们可以将 IP 利用 string 存起来，然后利用 `sscanf` 这一利器  
`sscanf` 的用法：`sscanf(源字符串,格式字符串,目标变量);`  
下面是一个示例，从 `date` 字符串中按照 `%d/%d/%d %d:%d:%d` 的格式，分别提取到变量 `year` `month` `day` `hour` `minute` `second` 中：  
`sscanf(date,"%d/%d/%d %d:%d:%d",&year,&month,&day,&hour,&minute,&second);`

`sscanf` 的返回值是 **成功提取变量的数量**，例如上面那个例子中的正确返回值应该是 6，如果返回值不等于 6，就说明提取失败。  
那么让我们带入到 IP 中，我们需要提取 5 个整数，只要返回值不是 5，立刻返回 `false` 即可  
又看到字符串的长度 $\le25$，所以最大的整数不超过 17 位，用 `long long` 存就可以。  
所以我们的一条判断条件就是：
`if(sscanf(ip.c_str(),"%lld.%lld.%lld.%lld:%lld",&a,&b,&c,&d,&e)!=5)  return false;`

现在我们提取到了每个整数，接下来就是判断是否合法了，依照题目判断范围即可，这一步不多说了

然后我们要解决前导 0 的问题。我们可以把我们提取出来的整数拼接回去，拼成一个正确的 IP 地址，然后和读入进来的 IP 进行比较，如果相同就证明没有前导 0。  
我们可以开一个 string，然后加起来就好了。但是因为你的整数化成字符串有点麻烦，你就可以使用 `to_string()` 函数，这属于 STL 的一种，在 C++11 被引入，OI 使用没问题。
`string now_ip=to_string(a)+'.'+to_string(b)+'.'+to_string(c)+'.'+to_string(d)+':'+to_string(e);`

---
验证 IP 解决了，下面就是连接问题。  
连接过程中，你需要根据客户端请求的 IP 寻找服务器 IP，然后输出对应的 ID 即可。  
这一步我们可以使用 `map` 或 `unordered_map` 来解决，这可以减少内存的占用。

我们建立 `unordered_map<string,bool> server_ip_exist` 来表示服务器 IP 是否存在，`unordered_map<string,int> server_id` 来表示服务器 IP 对应的 ID。  
然后在读入的时候先看看当前输入的 IP 是否存在，然后就可以标记存在和记录 ID 了。
### 代码
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
unordered_map<string,bool> server_ip_exist;
unordered_map<string,int> server_id;
bool check_ip(string ip)
{
    int a,b,c,d,e;
    if(sscanf(ip.c_str(),"%lld.%lld.%lld.%lld:%lld",&a,&b,&c,&d,&e)!=5)  return false;
    if(a<0||a>255||b<0||b>255||c<0||c>255||d<0||d>255||e<0||e>65535)  return false;
    string now;
    now=to_string(a)+'.'+to_string(b)+'.'+to_string(c)+'.'+to_string(d)+':'+to_string(e);
    if(now!=ip)  return false;
    return true;
}
signed main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        string type,ip;
        cin>>type>>ip;
        if(type=="Server")
        {
            if(server_ip_exist[ip])//服务器 IP 已存在
            {
                cout<<"FAIL"<<endl;
                continue;
            }
            if(!check_ip(ip))//IP 格式错误
            {
                cout<<"ERR"<<endl;
                continue;
            }
            server_ip_exist[ip]=true;//标记 IP 存在
            server_id[ip]=i;//记录 ID
            cout<<"OK"<<endl;
        }
        if(type=="Client")
        {
            if(!check_ip(ip))//IP 格式错误
            {
                cout<<"ERR"<<endl;
                continue;
            }
            if(!server_ip_exist[ip])//服务器 IP 不存在
            {
                cout<<"FAIL"<<endl;
                continue;
            }
            cout<<server_id[ip]<<endl;
        }
    }
    exit(0);
}
```
# 知识积累
## 与或非运算
这些操作都要在 **二进制下** 完成
- 按位与：`&`：每一位都必须是 `1` 才会得到 `1`，否则是 `0`
- 按位或：`|`：每一位只要有一个是 `1` 就会得到 `1`
- 按位非：`~`：每一位取反
- 按位异或：`^`：每一位相同为 `0`，不同为 `1`

在数学上，$\land$ 和 `&&` 是一样的，$\lor$ 和 `||` 是一样的，$\neg$ 和 `!` 是一样的  
这是 **逻辑运算符**，不要混淆
## （重难点）进制转换
转换技巧：遇到 x 进制转 y 进制，可以先把 x 进制转为 10 进制，再转为 y 进制
### x 进制转 10 进制
{% mark 按位展开，乘权相加 %}  
例如 $(2F)_{16}$ 转 10 进制：  
$$
({\color{red}2F})_{16}={\color{red}2}\times16^{\color{blue}1}+{\color{red}15}\times16^{\color{blue}0}=32+15=(47)_{10}
$$
在上面的式子中，$F$ 其实就是十进制里面的 $15$，这不用多说

如果是带小数的部分，要 {% mark 乘负权相加%}  
例如 $(A.F)_{16}$ 转 10 进制
$$
({\color{red}A.F})_{16}={\color{red}10}\times 16^{\color{blue}0}+{\color{red}15}\times 16^{\color{blue}-1}=10+\frac{15}{16^1}=(10.9375)_{10}
$$
### 10 进制转 x 进制
{% mark 不断除以 x，余数倒着写，商循环利用 %}   
例如 $(47)_{10}$ 转 16 进制
$$
47\div16={\color{blue}2}\dots 15({\color{red}F})\\
{\color{blue}2}\div16={\color{green}0}\dots {\color{red}2}\\
\therefore (47)_{10}=({\color{red}2F})_{16}
$$

如果是带小数的转换，整数部分还是和上面相同的，小数部分要 {% mark 不断乘以 x，商取整，顺着写，小数循环利用%}
例如 $(114.514)_{10}$ 转 8 进制  
那么我们先算整数部分：
$$
114\div 8={\color{blue}14}\dots {\color{red}2}\\
{\color{blue}14}\div 8={\color{blue}1}\dots {\color{red}6}\\
{\color{blue}1}\div 8={\color{green}0}\dots {\color{red}1}\\
\therefore (114)_{10}=({\color{red}162})_{8}
$$
然后算小数部分：
$$
0.514\times 8={\color{red}4}.{\color{blue}112}\\
{0.\color{blue}112}\times 8={\color{red}0}.{\color{blue}912}\\
{0.\color{blue}912}\times 8={\color{red}7}.{\color{blue}296}\\
\dots \\
\therefore (0.514)_{10}=(0.{\color{red}407})_{8}
$$
最后让我们加起来，得到了 $(114.514)_{10}=(162.407)_{8}$  
如果题目中需要更高的精度，那就继续乘以 8，直到小数部分为 0
### 常用数字的进制转换
|十进制|二进制|八进制|十六进制|
|:-:|:-:|:-:|:-:|
|0|0000|0|0|
|1|0001|1|1|
|2|0010|2|2|
|...|...|...|...|
|7|0111|17|7|
|8|1000|10|8|
|...|...|...|...|
|15|1111|17|F|
|16|10000|20|10|
|...|...|...|...|
## 二进制编码
- 原码：用 {% mark 最高位%} 作为符号位，0是正数，1是负数，符号位之后是这个十进制数的绝对值的二进制，存在 -0 的问题，所以不用
- 反码：正数和原码相同；负数符号位不变，剩余的 {% mark 在原码基础上按位取反 %}
- 补码：正数和原码相同；负数就是反码+1（忽略符号位进位）
## GCD 和 LCM
- GCD：Greatest Common Divisor，最大公约数
- LCM：Least Common Multiple，最小公倍数
- GCD 和 LCM 之间的关系：
$$
GCD(a,b)\times LCM(a,b)=a\times b
$$
- 在 C++14 标准之后（OI 可用），你可以通过 `algorithm` 库中的 `std::__gcd(int,int)` 和 `std::__lcm(int,int)` 来计算 GCD 和 LCM
- 在 CSP 一轮考场上，可以直接带入选项中的数值去尝试
## （重难点）排列与组合
### 排列
概念：集合 $S$ 由 $n$个不同的元素组成，取出其中的 $r$ 个，{% mark 与顺序有关 %} 的情况数为 $A_n^r$，计算公式为：
$$
A_n^r=\frac{n!}{(n-r)!}
$$
例如从 5 个元素中选出 2 个，求有多少种方案（数字相同但顺序不同也算另一种）  
那么一共有
$$
A_5^2=\frac{5!}{(5-2)!}=\frac{5\times4\times3\times2\times1}{3\times2\times1}=\frac{120}{6}=20
$$
种方案

当 $n=r$ 时，$A_n^r=A_n^n=n!$
### 组合
概念：集合 $S$ 由 $n$个不同的元素组成，取出其中的 $r$ 个，{% mark 与顺序无关 %} 的情况数为 $C_n^r$，计算公式为：
$$
C_n^r=\frac{A_n^r}{r!}=\frac{\frac{n!}{(n-r)!}}{r!}=\frac{n!}{r!(n-r)!}
$$
例如从 5 个元素中选出 2 个，求有多少种方案（数字相同但顺序不同只算做同一种）  
那么一共有
$$
C_5^2=\frac{5!}{2!(5-2)!}=\frac{5\times4\times3\times2\times1}{2\times1\times3\times2\times1}=\frac{120}{2\times6}=10
$$
种方案

当 $n=r$ 时，$C_n^r=C_n^n=1$
### 排列与组合的联系
$$
A_n^r=C_n^r\times A_r^r\\
C_n^r=C_n^{n-r}\\
C_{n+1}^r=C_n^r+C_n^{r-1}
$$
### 有重复的排列
给出具有 $n$ 个对象的集合 $S$，选出 $r$ 个，可以重复选择  
从第 1 个位置到第 $r$ 个位置，每个位置有 $n$ 种选择，那么一共有 $n^r$ 种方案
### 有重复的组合（插板法）
给出具有 $n$ 个对象的集合 $S$，选出 $r$ 个，可以重复选择，与顺序无关  
最终方案数是 $C_{n+r-1}^r$
## （重难点）小球与盒子
你有 $n$ 个小球，要放到 $m$ 个盒子里，每个盒子可以放多个小球，
### 可辨别的小球与可辨别的盒子
最终方案数是 $C_{n+m-1}^n$
### 不可辨别的小球与可辨别的盒子
盒子可以是空的：$C_{n+m-1}^{m-1}$
盒子不能是空的：$C_{n-1}^{m-1}$

剩下两种严重超纲，不记（涉及到第二类斯特林数和很多的分类讨论）
## （易考）树的遍历
给出前序遍历和中序遍历，可以唯一确定一棵二叉树  
给出后序遍历和中序遍历，可以唯一确定一棵二叉树
### 步骤
1. 前序遍历的第一个元素是根节点，后序遍历的最后一个元素是根节点
2. 在中序遍历中找到根节点，它的左边是左子树，右边是右子树
3. 递归构建左子树和右子树
### 示例
前序：`ABDECFG`  
中序：`DBEAFCG`

1. 首先确定 `A` 为根节点
2. 在中序遍历中找到 `A`，它的左边 `DBE` 是左子树，右边 `FCG` 是右子树
3. 在前序 `BDE` 和 `CFG` 中得到后序遍历 `DBE` 和 `CFG`
4. 接上根节点 `A`，得到后序遍历 `DBECFGA`

也可以去 2019 CSP-J1 T14 练习
## （易考）单位换算
`M = 1e6`，`K = 1e3`  
`1B(yte) = 8bit`
### 二进制存储空间换算
`1TiB = 2e10GiB = 2e20MiB = 2e30KiB = 2e40B = 2e43bit`  
$1 TiB = 2^{10} GiB = 2^{20} MiB = 2^{30} KiB = 2^{40} B = 2^{43} bit$
### 十进制存储空间换算
`1TB = 1e3GB = 1e6MB = 1e9KB = 1e12B = 8*1e12bit`  
$1 TB = 10^3 GB = 10^6 MB = 10^9 KB = 10^{12} B = 8\times10^{12} bit$
## 文件存储空间
图片存储空间：$分辨率\times 位深度(bit)$
视频存储空间：$单张图大小\times fps\times 时长(bit)$
            $(视频 bps+音频 bps)\times 时长$（单位根据码率单位而定）
帧率缩写 fps，码率 bps
## NOI 历史
- 第一届 NOI：1984，今年（NOI2025）第 41 届
- 第一届 IOI：1989
- 我国 2000 举办第 12 届 IOI
- 第一届 NOIP：1995，2019 暂停一届
- 第一届 CSP：2019
## 网络
### OSI 七层模型
1. 物理层：光纤、中继器
2. 数据链链路层：网卡、交换机、以太网
3. 网络层：IP、ICMP
4. 传输层：TCP、UDP
5. 会话层：SSL、TLS
6. 表示层：LPP
7. 应用层：HTTP、FTP、SMTP、POP3
### 协议
HTTP：超文本传输协议
FTP：文件传输协议
SMTP：收发电子邮件
POP3：接收电子邮件
TCP：三次握手(A->B 质询在线，B->A 返回在线，A->B 返回确认收到)
### IP 地址
IPv4：`x.x.x.x`（0<=x<=255）32bit 4B
- A类地址：`1.0.0.1` ~ `127.255.255.254`
- B类地址：`128.0.0.1` ~ `191.255.255.254`
- C类地址：`192.0.0.1` ~ `223.255.255.254`

IPv6：`X:X:X:X:X:X:X:X`（0000<=X<=FFFF）64bit 8B
## 排序
### 算法信息
n=等待排序的个数，w=数据值域
|算法名称|平均时间|最坏时间|是否稳定|
|:-:|:-:|:-:|:-:|
|冒泡排序|$O(n^2)$|$O(n^2)$|true|
|选择排序|$O(n^2)$|$O(n^2)$|false|
|插入排序|$O(n^2)$|$O(n^2)$|true|
|计数排序|$O(n+w)$|$O(n+w)$|true|
|归并排序|$\theta(n log n)$|$\theta(n log n)$|true|
|快速排序|$O(n log n)$|$O(n^2)$|false|
### 快速排序原理
使用递归原理  
在 a[l]~a[r] 中选择一个基准 x  
使用双指针将所有 `a[i] < x` 交换到 x 左侧，`a[i] > x` 交换到 x 右侧  
快速排序通过双指针交换后，将一个大的排序问题划分为了两个子区间排序的小问题  
所以期望复杂度 $O(n log n)$  
如果选择第一个数作为基准，序列有序，每次序列长度只减少 1  
所以最坏复杂度 $O(n^2)$
### std::sort
由 algorithm 头文件提供，属于 STL  
在数据范围较小的情况下，使用了堆排序等技术
## 数据结构
### 链表
优点：插入删除 $O(1)$  
缺点：读取 $O(n)$ n=链表元素个数

示例：
```
[prev = NULL, id = 1, value = 114, next = 514]  
[prev = 1, id = 514, value = 1919, next = 1919]  
[prev = 514, id = 1919, value = 810, next = NULL]
```
### 图的存储
#### 邻接矩阵
优点：查询边权 $O(1)$
缺点：对于稀疏图，浪费空间
#### 邻接表
优点：空间复杂度 $O(|E|)$
缺点：查询边权 $O(|V|)$
### 树
有 $n$ 个节点 $n-1$ 条边的无向连通图
完整二叉树：每个节点的子节点数量都是 0 或 2
## 运算优先级
自增/自减 执行顺序

`++x` 先增加 x 的值，然后送出使用  
`x++` 先送出使用，然后增加 x 的值  

{% copy ans+=++x; = x=x+1;ans=ans+x; prefix:C++%}
{% copy ans+=x++; = ans=ans+x;x=x+1; prefix:C++%}

`x++ + ++x`  
这属于 未定义行为（Undefined Behavior, UB），在不同的编译器上可以解释为任何值。  
这可能导致本地通过，评测报错  
可以通过 `-Wall` 参数找到所有的警告
## 计算原理
### 容斥原理/减法法则
如果一个任务可以通过 $n_1$ 种方法执行，还可以通过 $n_2$ 种另一类方法执行，那么执行这个任务的方法数量是 $n_1+n_2-[两种方法中相同的方法]$  
也就是说，集合 $A_1,A_2$，与并集 $A_1\cup A_2$，满足 $|A_1\cup A_2|=|A_1|+|A_2|-|A_1\cap A_2|$  
### 鸽巢原理
如果 $k+1$ 个或更多的物体放入 $k$ 个盒子，那么至少有一个盒子包含了 2 个或更多的物体
### 广义鸽巢原理
如果 $n$ 个物体放入 $k$ 个盒子，那么至少有一个盒子至少包含了 $\lceil \frac{n}{k}\rceil$ 个物体
## 高级语言分类
- 面向过程：以函数为基本程序结构：C，Pascal，Fortran
- 面向对象：以类为基本程序结构：C++，Java，Python
- 编译性：执行前用连接器生成可执行文件：C++
- 解释性：一边用解释器翻译，一边运行代码：Python，JS，Ruby
- Java：编译 + 解释混合
## 计算机历史
- 第一台计算机 - ENIAC
- 有存储功能 - EDVAC
- 冯诺依曼 - 存储结构
- 图灵 - 测试
- 阿达罗福莱斯 - 计算机程序的创始人
- 马文·明斯基、约翰·麦卡西 - 对 AI 杰出贡献
- 香农 - 信息论
## 哈夫曼树
// TODO
