---
title: Queue, Stack, Vector의 push pop 연산 시간
date: 2020-08-22 22:00:00 +0800
categories: [Algorithm]
tags: [c++]
seo:
  date_modified: 2020-09-03 20:15:08 +0900
---

# 큐, 스택은 느리다  
---
큐와 스택의 `push`와 `pop` 연산에는 `O(1)`의 시간이 걸린다.  
그렇다면 1억번의 `push`를 한다면 1초가 걸릴까?  
나는 그렇게 알고있었지만 아니다.  
큐와 스택은 생각보다 훨씬 느린 아이였다.  

```c++
#include<iostream>
#include<time.h>
#include<queue>
using namespace std;

int main() {
	queue<int>q;
	clock_t st = clock();
	for (int i = 0; i < 1000000; i++) {
		q.push(1);
		q.pop();
	}
	clock_t end = clock();
	printf("%lfs", (double)(end - st) / CLOCKS_PER_SEC);
	return 0;
}
```
큐의 `push`와 `pop` 연산의 시간을 재기 위해서 짠 코드이다.  
`push`와 `pop`을 각각 백만번씩 수행했다.  
위 코드의 결과는 `1.441000s`이다.  
무려 1.4초나 걸렸다.  
<br>

위와 똑같은 코드로 Stack, Deque도 돌려보았지만  
마찬가지로 비슷하게 1.5초 정도 걸렸다.  
이것과 관련해서 열심히 찾아보았지만 왜 그런지 찾을 수 없었다.  

```c++
#include<iostream>
#include<time.h>
#include<queue>
using namespace std;

int main() {
	queue<int>q;
	clock_t st = clock();
	for (int i = 0; i < 1000000; i++) {
		q.push(1);
	}
	clock_t end = clock();
	printf("%lfs", (double)(end - st) / CLOCKS_PER_SEC);
	return 0;
}
```
위 코드처럼 단순히 `push` 연산만 하는 코드는 0.9초 정도 걸린다.  
이것은 동적할당 때문에 오래걸릴 수 있다고 생각되는데  
`push`와 `pop`을 동시에 하는 위의 코드는 왜 오래걸리는지 모르겠다.  
<br>

궁금해서 벡터도 해보았는데  
```c++
#include<iostream>
#include<time.h>
#include<vector>
using namespace std;

int main() {
	vector<int>v;
	clock_t st = clock();
	for (int i = 0; i < 1000000; i++) {
		v.push_back(1);
		v.pop_back();
	}
	clock_t end = clock();
	printf("%lfs", (double)(end - st) / CLOCKS_PER_SEC);
	return 0;
}
```
위의 코드의 결과는 `0.480000s`이다.  
벡터가 더 빠르다니 스택이 느린게 이해가 안간다...

아무튼 Stack, Queue, Deque 등을 사용할 때는  
`push`와 `pop` 연산이 엄청 오래걸려 주의해야 한다는 것이다.