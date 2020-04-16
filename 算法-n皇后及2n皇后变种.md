# 算法-n皇后及2n皇后变种

## 题意

> 在给定正方形棋盘上，放置n个皇后，要求皇后不可以在同行、同列、同一的斜线，求出可以放置的总方法数

## 思路

> 题目采用递归求解，回溯法。  
> 每次从一行开始从头到尾，挨个遍历，尝试每一列是否可行，可以就进入下一行求解。同时记录下来放置的列数，这样一行一行下去就不需要验证是否在同一行，同时使用一个数组存下每一行放置的列数，即可方便快捷.  
> 同一斜线的验证，使用数组存放后正好去验证，行坐标与纵坐标的差值的绝对值是否相同，高效验证，还使用到了数组存放的优势。

## 变种

> 在处理问题时，把最终结束递归处，改为开始下一次递归的入口，从第一行开始检索，验证，最终求出解答。入口处先复制下来(memcpy)，然后挨个将放置的位置置为无效，开始新的遍历。

## 代码

```C++
#include<bits/stdc++.h>
using namespace std;
int arr1[100][100] = {0};
int arr2[100][100] = {0};
int put1[100]={0}; 
int put2[100]={0}; 
int count1 = 0;
int count2 = 0;
void search2(int n,int lenth){
	if(n == lenth){
		count2++;
	}else{
		for(int i =0;i<lenth;i++){
			if(arr2[n][i] == 1){
				int k =0;
				for(k;k<n;k++){
					if(put2[k] == i || abs(k - n) == abs(put2[k] - i)) break;
				}
				if(k == n){
					put2[n] = i;
					search2(n+1,lenth);
				}
			}
		}
	}
}

void search1(int n,int lenth){
	if(n == lenth){
		memcpy(arr2,arr1,sizeof(arr1));
		for(int i =0;i<lenth;i++){
			arr2[i][put1[i]] = 0;
		}
		search2(0,lenth);
		count1++;
	}else for(int i=0;i<lenth;i++){
		if(arr1[n][i] == 1){
//			表示此处可以放置皇后
			int k =0;
			for(k;k<n;k++){
				if(abs(put1[k] - i) == abs(k - n) || put1[k] == i) break;
			}
			if(k == n){
				put1[n] = i;
				search1(n+1,lenth);
			}
		}
	}
}

int main(){
	int n;
	cin >> n;
	for(int i=0;i<n;i++){
		for(int k=0;k<n;k++){
			cin >> arr1[i][k];
		}
	}
	search1(0,n);
	cout << count2 << endl;
//	cout << count1 << " " << count2 << endl;
	return 0;
}
```