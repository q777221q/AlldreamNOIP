﻿# 逆序对

## 题目描述    

猫猫TOM和小老鼠JERRY最近又较量上了，但是毕竟都是成年人，他们已经不喜欢再玩那种你追我赶的游戏，现在他们喜欢玩统计。最近，TOM老猫查阅到一个人类称之为“逆序对”的东西，这东西是这样定义的：对于给定的一段正整数序列，逆序对就是序列中`a_i > a_j`且`i<j`的有序对。知道这概念后，他们就比赛谁先算出给定的一段正整数序列中逆序对的数目。  

## 输入格式  

第一行，一个数n，表示序列中有n个数。  
第二行n个数，表示给定的序列。序列中每个数字不超过10^9  

## 输出格式  

给定序列中逆序对的数目。     

## 输入样例  
```	 
6  
5 4 2 6 3 1   
```    

## 输出样例  
```		
11    
```   

## 测试网站  	
[p1908](https://www.luogu.org/problemnew/show/P1908)  	 

## 题目分析  	
这道题有很多方法可以理解，我这里用的树状数组+离散化。  
假设给定的序列为 4 3 2 1  
我们从左往右依次将给定的序列输入，每次输入一个数i时，就将已输入序列中大于i的元素的个数计算出来，并累加到ans中，最后ans就是这个序列的逆序数个数  
当所有的元素都插入到序列后，即可得到序列{4 3 2 1}的逆序数的个数为1+2+3=6.  
例如求第三个位置的逆序对数时：ans += 3-sum(3)；sum(3)得到小于等于3的数出现的次数；3为当前一共出现的数的个数  

## 代码示例  

```c++	
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
#define N 500010
#define ll long long
ll c[N];//C[]为树状数组
int a[N], h[N];//a[]为读入的数组，h[]为要离散化后的数组
int n;
int lowbit(int x)
{
    return x&(-x);
}
void Insert(int i,int x)
{
    while(i<=n){
        c[i]+=x;
        i+=lowbit(i);
    }
}
ll getsum(int i)//树状数组求和
{
    ll sum=0;
    while(i>0){
        sum+=c[i];
        i-=lowbit(i);
    }
    return sum;
}
int main()
{
        cin>>n;
        ll ans=0;
        memset(c,0,sizeof(c));
        for(int i=1;i<=n;i++){
            cin >> a[i];
            h[i] = a[i];
        }
        sort(h+1, h+n+1);
        int t = unique(h+1, h+n+1) - h;//离散化
        for(int i=1; i<=n; i++){
            int k = lower_bound(h+1, h+t, a[i]) - h;
            Insert(k,1);//将当前元素插入树状数组，维护当前的状态
            ans+=i-getsum(k);//统计当前序列中大于a[i]的个数
        }
        cout<<ans<<endl;
    return 0;
}
```