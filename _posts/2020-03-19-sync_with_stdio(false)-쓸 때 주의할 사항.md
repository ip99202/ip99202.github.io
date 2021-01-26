---
title: sync_with_stdio(false) 쓸 때 주의할 사항
date: 2020-03-26 16:35:00 +0800
categories: [Algorithm]
tags: [algorithm]
seo:
  date_modified: 2021-01-19 15:55:35 +0900
---

## 주의할 점
백준 문제를 풀다가 아래 코드가 틀렸었다.  
왜 틀렸나 봤는데 `ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);`이 부분 때문이었다.  
```c++
#include<iostream>
#include<string>
using namespace std;
int arr[53];
int ans[53];
string a;

int main() {
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		int x;
		cin >> x;
		arr[x]++;
	}
	getchar();
	getline(cin, a);
	for (int i = 0; i < n; i++) {
		if (a[i] >= 'A' && a[i] <= 'Z')
			ans[a[i] - 'A' + 1]++;
		else if (a[i] >= 'a' && a[i] <= 'z')
			ans[a[i] - 'a' + 27]++;
		else if (a[i] == ' ')
			ans[0]++;
	}
	bool flag = true;
	for (int i = 0; i < 53; i++) {
		if (arr[i] != ans[i])
			flag = false;
	}
	if (flag == false)
		cout << 'n';
	else
		cout << 'y';
}
```

`ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);`  
이 코드는 C와 C++의 표준 stream의 동기화를 끊는 역할을 한다.  
cin과 cout의 속도가 C의 입출력 속도에 비해 떨어지기 때문에 저 코드를 사용해 속도를 높이는 기능으로 사용한다.  
자세한 내용은 [이 글](https://ip99202.github.io/posts/%EC%9E%85%EC%B6%9C%EB%A0%A5-%EC%86%8D%EB%8F%84-%EC%A4%84%EC%9D%B4%EA%B8%B0/)을 참고  
하지만 동기화를 끊게되면 C의 입출력 함수를 더 이상 사용하지 못하는데  
그 동안 printf와 scanf만 주의하면 된다고 생각했었는데 getchar도 C에서 쓰이는 입출력 함수이다.  
그렇기 때문에 위에 getchar를 사용하지 못해 위의 코드는 틀렸습니다가 나오는 것이다.  
비쥬얼스튜디오에서는 동기화를 끊어도 섞어쓰면 알아서 처리해주는 것 같지만 채점프로그램에서는 그렇지 않다.  

그렇기 때문에 항상 `ios::sync_with_stdio(false);`를 쓴다면 C의 입출력을 쓰지않도록 조심해야한다.  
위의 틀린 코드는 
```c++
#include<iostream>
#include<string>
using namespace std;
int arr[53];
int ans[53];
string a;

int main() {
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		int x;
		cin >> x;
		arr[x]++;
	}
	getline(cin, a);
	getline(cin, a);
	for (int i = 0; i < n; i++) {
		if (a[i] >= 'A' && a[i] <= 'Z')
			ans[a[i] - 'A' + 1]++;
		else if (a[i] >= 'a' && a[i] <= 'z')
			ans[a[i] - 'a' + 27]++;
		else if (a[i] == ' ')
			ans[0]++;
	}
	bool flag = true;
	for (int i = 0; i < 53; i++) {
		if (arr[i] != ans[i])
			flag = false;
	}
	if (flag == false)
		cout << 'n';
	else
		cout << 'y';
}
```
이런식으로 getline을 한번 더 써서 개행문자를 처리하도록 했다.  
위의 코드는 정답처리 되었다.