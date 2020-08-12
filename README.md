# ceiler.github.io
### 题目链接 <https://pintia.cn/problem-sets/994805342720868352/problems/994805523835109376>
### 题目描述：As an emergency rescue team leader of a city, you are given a special map of your country. The map shows several scattered cities connected by some roads. Amount of rescue teams in each city and the length of each road between any pair of cities are marked on the map. When there is an emergency call to you from some other city, your job is to lead your men to the place as quickly as possible, and at the mean time, call up as many hands on the way as possible.


###  输入要求：Each input file contains one test case. For each test case, the first line contains 4 positive integers: N (≤500) - the number of cities (and the cities are numbered from 0 to N−1), M - the number of roads, C​1​​ and C​2​​ - the cities that you are currently in and that you must save, respectively. The next line contains N integers, where the i-th integer is the number of rescue teams in the i-th city. Then M lines follow, each describes a road with three integers c​1​​, c​2​​ and L, which are the pair of cities connected by a road and the length of that road, respectively. It is guaranteed that the/re exists at least one path from C​1​​ to C​2​​.

### 输出要求：For each test case, print in one line two numbers: the number of different shortest paths between C​1​​ and C​2​​, and the maximum amount of rescue teams you can possibly gather.  All the numbers in a line must be separated by exactly one space, and there is no extra space allowed at the end of a line.

### 样例：


5 6 0 2
1 2 1 5 3
0 1 1
0 2 2
0 3 1
1 2 1
2 4 1
3 4 1


输出

2 4


### 分析：题目主要需要考虑求出到目标城市的最短路径的数量，考虑使用Dijkstra求出最短路，观察到本题又赋予了点权，所以在Dijkstra中可以直接进行点权的计算。
### 考虑到本题数据量为500，故使用朴素版Dijkstra。
### 以下是代码部分

```cpp
#include<iostream>
#include<cstdio>
#include<queue>
#include<vector>
#include<algorithm>
#include<cstring>
using namespace std;
const int N=1005;
int g[N][N];//记录边权
bool st[N];//记录点的访问情况
int n,m,sta,en;
int sum[N];//记录点权之和，即医疗队的人数
int path[N];//记录从起点到终点的最短路数量
int dist[N];//最短路
int r[N];//记录点权
void dijkstra()
{
    sum[sta]=r[sta];//进行初始化
    path[sta]=1;//考虑到一个城市
    memset(dist,0x3f,sizeof dist);//初始化dist
    dist[sta]=0;
    for(int i=0;i<n;i++)
    {
        int t=-1;
        for(int j=0;j<n;j++)
            if(!st[j]&&(t==-1||dist[t]>dist[j]))
                t=j;
        st[t]=true;
        if(t==-1)break;
        for(int j=0;j<n;j++)
        {
            if(dist[j]>dist[t]+g[t][j])
            {
                sum[j]=sum[t]+r[j];
                path[j]=path[t];
            }
            else if(dist[j]==dist[t]+g[t][j])
            {
                path[j]+=path[t];
                sum[j]=max(sum[j],sum[t]+r[j]);
            }
            dist[j]=min(dist[j],dist[t]+g[t][j]);
        }
    }
}
int main()
{
    memset(g,0x3f,sizeof g);//初始化边权
    scanf("%d%d%d%d",&n,&m,&sta,&en);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&r[i]);
    }
    for(int i=0;i<m;i++)
    {
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        if(g[a][b]>c)
            g[a][b]=g[b][a]=c;//此处重要，本题为有向有环图；只记录两点之间最小边权
    }
    dijkstra();
    cout<<path[en]<<' '<<sum[en];
    return 0;
}

```

