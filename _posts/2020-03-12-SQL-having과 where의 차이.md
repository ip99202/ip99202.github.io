---
title: SQL having과 where의 차이
date: 2020-03-13 19:00:00 +0800
categories: [Develop, SQL]
tags: [sql]
seo:
  date_modified: 2020-03-16 00:14:23 +0900

---

## where
```sql
select * from 테이블명 where 조건절
```
항상 from 뒤에 존재하며 비교연산자들을 사용해 조건을 줄 수 있다.


## having
```sql
select * from 테이블명 group by 필드명 having 조건절
```
항상 group by 뒤에 존재하며 마찬가지로 비교연산자들을 사용해 조건을 준다.


## 차이점과 공통점
둘다 조건을 주는 명령어이지만  
where 같은 경우는 모든 필드에 대해 우선적으로 조건을 주고  
having은 group by 된 이후 그룹화되어진 새로운 테이블에 조건을 줄 수 있다는 차이점이 있다.  
한마디로 having은 group by와 엮인 명령어 다른곳에서 쓰이는 일은 없을 것 같다.  

예를들어 동물 테이블이 있고 이름이 존재하고 2번 이상 나온 이름들을 그룹짓고 싶다면
```sql
select NAME, count(NAME) as count
from ANIMAL_INS
group by NAME
having NAME is not null and count(NAME) > 1;
```
그룹을 지은 후 having을 사용해 조건을 줘야 한다.