---
title: 유니온 파인드(Union-Find)
date: 2020-07-16 14:30:00 +0800
categories: [Algorithm, Union-Find]
tags: [Union-Find]
seo:
  date_modified: 2020-07-16 16:27:27 +0900

---

## 유니온 파인드란?  
- 유니온 파인드는 그래프 알고리즘으로 두 노드가 같은 그래프에 속하는지 판별하는 알고리즘이다.  
- 서로소 집합, 상호 베타적 집합(Disjoint-Set)으로도 불린다.  
- 노드를 합치는 Union연산과 노드의 루트 노드를 찾는 Find연산으로 이루어진다.  
- 트리 구조로 이루어진 자료구조 중 한가지로 생각되기도 한다.  
<br>

![image](/assets/img/postImg/unionfind/unionfind01.PNG)  
이렇게 생긴 기차 노선이 있다고 생각해보자  
기차를 타고 서울에서 강릉으로 가는건 가능하지만 서울에서 부산으로 가는건 불가능하다.  
간단한 그래프에서는 한점에서 다른점으로 가는 길이 이어져있는지 판단하는게 간단하다.  
<br>
![image](/assets/img/postImg/unionfind/unionfind02.PNG)  
하지만 이런 복잡한 그래프에서 빠른 시간안에 판별하는건 힘들다.  
이렇게 복잡한 그래프에서 두 노드가 연결되어 있는지 단절되어 있는지 판별하기 위한 알고리즘을 알아보자.  
<br><br>

## 유니온 파인드  
![image](/assets/img/postImg/unionfind/unionfind03.PNG)  
8개의 단절된 노드들이 있다.  
이 노드들은 배열에 자기 자신의 값을 가지고 있다.  
<br><br>
![image](/assets/img/postImg/unionfind/unionfind04.PNG)  
이 중에 1 2 노드를 연결하고 4 5 6 노드들을 연결하고 싶다.  
<br><br>
![image](/assets/img/postImg/unionfind/unionfind05.PNG)  
우선 1 2 노드를 합쳐보자  
합치는 방법은 간단하다 2번 노드의 배열 값을 1번 노드의 배열 값으로 바꿔주면 된다.  
<br><br>
![image](/assets/img/postImg/unionfind/unionfind06.PNG)  
4 5 6도 마찬가지로 바꿔보자  
4와 5를 연결하고 5와 6을 연결하면 그림처럼 배열이 바뀐다.  

이제 어떻게 노드들이 연결되어 있는지 아닌지 판별할 수 있는지 감이 왔을것이다.  
예를들어 2번과 6번 노드가 연결되어 있는지 확인하고 싶다면  

- 2번 노드의 부모를 찾는 과정
1. 배열의 2번에 있는 값을 확인한다. 1번이다.
2. 배열의 1번에 있는 값을 확인한다. 1번이다.
3. 배열의 인덱스와 값이 일치한다.
4. 그렇다면 1번 노드가 2번 노드가 속해있는 트리의 루트 노드이다.
 
- 6번 노드의 부모를 찾는 과정
1. 배열의 6번에 있는 값을 확인한다. 5번이다.
2. 배열의 5번에 있는 값을 확인한다. 4번이다.
3. 배열의 4번에 있는 값을 확인한다. 4번이다.
4. 배열의 인덱스와 값이 일치한다.
5. 4번 노드가 6번 노드가 속해있는 트리의 루트 노드이다.

이제 각각 노드의 루트 노드를 비교해서 서로 다르다면 두 노드는 연결되어 있지 않은 것이다.  

하지만 이렇게만 바꾸면 문제가 생긴다.  
<br><br>
![image](/assets/img/postImg/unionfind/unionfind07.PNG)  
위의 예처럼 트리의 문제 중 하나인 한쪽으로만 자식이 몰려있는 편중현상이 생길 수 있는 것이다.  
Find연산에서 재귀를 통해 루트 노드를 찾아야하는데 이렇게 편중되어 있으면 탐색하는데 시간이 많이 걸린다.  
<br><br>
![image](/assets/img/postImg/unionfind/unionfind08.PNG)  
위의 편중 문제를 해결하기 위해 합치는 Union연산을 할 때  
루트 노드를 찾는 탐색과정을 통해 루트노드에 연결하는 과정을 거쳐  
결과적으로 위의 그림처럼 트리가 형성된다.  
<br><br>
![image](/assets/img/postImg/unionfind/unionfind09.PNG)  
1 2 를 연결하고 4 5 6을 연결한 유니온 파인드의 트리구조이다.  
<br><br>
![image](/assets/img/postImg/unionfind/unionfind10.PNG)  
이제 1과 4번 노드를 연결하고 싶다면 위의 그림처럼 바뀔 것이다.  
위의 과정을 다 이해할 수 있다면 유니온 파인드를 다 이해한것이다.  
<br><br>

## 유니온 파인드 코드
```c++
#include<iostream>
using namespace std;
int parent[9]; // 위 그림에서 배열에 해당하는 것

int find(int x) {
	if (parent[x] == x) // 배열 인덱스와 값이 같다면 해당 값 리턴
		return x;
	return parent[x] = find(parent[x]); // 배열의 값을 인덱스로 갖는 값 리턴
}

void merge(int x, int y) {
	x = find(x);
	y = find(y); // 각각 find연산을 통해 루트 노드를 가짐
	if (x == y) return; // x와 y가 같다면 이미 연결되어있는 것
	parent[y] = x; // 배열의 y인덱스에 x값을 저장
}

bool isUnion(int x, int y) { // 두 노드가 연결되어있는지 판별하는 함수
	x = find(x);
	y = find(y);
	if (x == y)
		return true;
	return false;
}

int main() {
	for (int i = 1; i <= 8; i++) // 배열 초기화 과정
		parent[i] = i;

	merge(1, 2);
	merge(4, 5);
	merge(5, 6);
	cout << "2와 4는 같은 집합인가?\n" << isUnion(2, 4) << "\n"; // false
	merge(1, 5);
	cout << "2와 4는 같은 집합인가?\n" << isUnion(2, 4) << "\n"; // true
	return 0;
}
```