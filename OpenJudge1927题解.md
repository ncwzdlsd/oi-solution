---
title: OpenJudge1927题解
date: 2022-11-24 18:52:22
url: solution-openjudge1927
categories: 题解
tags: 高精度
---

- 为方便计算，把 $c$ 从 $1$ 开始

- $n$ 为该数的位数

- 将该位之前的余数与该位数构成新的被除数

- 求 $c\bmod i$ 的倍数

- 如果余数 $=0$，说明 $c$ 是 $i$ 的倍数

```cpp
#include <bits/stdc++.h>
using namespace std;

char c[35];
int n,i,k,t,flag;

int main()
{
	scanf("%s",c+1);
	n=strlen(c+1);
	for(k=2;k<=9;k++)
	{
		t=0;
		for(i=1;i<=n;i++) t=t*10+c[i]-'0',t%=k;
		if(t==0) flag=1,cout<<k<<' ';
	}
	if(flag) cout<<endl;
	else cout<<"none";
	return 0;
}
```
