# 蓝桥杯-sin之舞

## 概要

> 　　最近FJ为他的奶牛们开设了数学分析课，FJ知道若要学好这门课，必须有一个好的三角函数基本功。所以他准备和奶牛们做一个“Sine之舞”的游戏，寓教于乐，提高奶牛们的计算能力。
　　不妨设
　　An=sin(1–sin(2+sin(3–sin(4+...sin(n))...)
　　Sn=(...(A1+n)A2+n-1)A3+...+2)An+1
　　FJ想让奶牛们计算Sn的值，请你帮助FJ打印出Sn的完整表达式，以方便奶牛们做题。

## 思路

> 刚开始使用的是循环打印，可以  
> 后来看待提示说是，递归，然后尝试了，在昨晚(2020年4月19日13:40:37)，在顺寻问题卡了很久，因为没有适当的策略对应对递增，还是递减递归  
> 总结——观察规律，看主要矛盾序号决定，是递增还是递减递归。  递增时需要穿两个参数，一个是序号，另一个是总量，用于判断；递减则只需要一个即可。出口条件置为1；  

## 代码

```C++
#include<bits/stdc++.h>
using namespace std;

int n =0;

string tostring(int n){
	string ret  ="";
	do{
		ret = char(n%10 + '0') + ret;
		n /= 10;
	}while(n>0);
	return ret;
}

// makesin num 第几个,从1开始，到total结束递归 得出的是an（n = total） 
string makesin(int num,int total){
	if(num == total){
		return "sin(" + tostring(total) + ')';
	}else{
		if(num % 2){
			return "sin(" + tostring(num) + '-' + makesin(num+1,total) + ')';
		}else{
			return "sin(" + tostring(num) + '+' + makesin(num+1,total) + ')';
		}
	}
}

// 参数num 指的是sn的值 
string makesn(int num){
	if(num == 1){
		return makesin(1,1) + '+' + tostring(n);
	}else{
		return '('+ makesn(num-1) + ')'+ makesin(1,num) + '+' + tostring(n+1-num);
	}
}

int main(){
	cin >> n;
	cout << makesn(n);
	return 0;
}
```
