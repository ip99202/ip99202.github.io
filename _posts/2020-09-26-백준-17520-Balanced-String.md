---
title: 백준 17520 Balanced String c++
date: 2020-09-25 19:00:00 +0800
categories: [Algorithm, BOJ]
tags: [boj]
---

## 문제
[https://www.acmicpc.net/problem/17520](https://www.acmicpc.net/problem/17520)  
<br>

## 문제해설  
n이 주어지면 길이가 n인  
이진수들 중에 0과 1의 개수 차이가  
1이하인 수의 개수를 출력해주면 된다.
<br>

## 문제풀이  
직접 1부터 해보다보면 규칙성을 발견할 수 있다.  
짝수에서 홀수로 증가할 때는 경우의 수가 2배로 커지고  
홀수에서 짝수로 증가할 때는 경우의 수가 그대로이다.  

예를들어 n이 3이라면 가능한 수들은  
010, 100, 101, 011 4개이고  
n이 4가 된다면  
0과 1의 개수가 완전히 같아져야하기 때문에  
0101, 1001, 1010, 0110으로 뒤에 0 또는 1만 붙게된다.  
하지만 n이 5가 되는 순간  
각 수에는 0 또는 1이 둘 다 붙을 수 있다.  
왜냐하면 홀수에서는 어쩔수없이 0과 1의 개수가 1차이가 나기 때문이다.  
<br>


## 전체코드
```c++
#include<iostream>
#include<algorithm>
#define mod 16769023
using namespace std;

int main() {
	ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	int n; cin >> n;
	int sum = 2;
	for (int i = 2; i <= n; i++) {
		if (i % 2 == 1)
			sum = (sum * 2) % mod;
	}
	cout << sum;
	return 0;
}
```