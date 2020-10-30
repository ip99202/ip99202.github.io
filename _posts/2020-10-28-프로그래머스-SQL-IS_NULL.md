---
title: 프로그래머스 SQL 고득점 Kit IS NULL
date: 2020-10-28 18:00:00 +0800
categories: [Develop, SQL]
tags: [프로그래머스, sql]
---

[프로그래머스 SQL IS NULL](https://programmers.co.kr/learn/courses/30/parts/17045)  

# 정답

## 1. 이름이 없는 동물의 아이디  
```sql
select ANIMAL_ID from ANIMAL_INS 
where NAME is NULL
order by ANIMAL_ID asc;
```

## 2. 이름이 있는 동물의 아이디  
```sql
select ANIMAL_ID from ANIMAL_INS
where NAME is not null
order by ANIMAL_ID asc;
```

## 3. NULL 처리하기  
```sql
select ANIMAL_TYPE, ifnull(NAME, 'No name') as NAME, SEX_UPON_INTAKE
from ANIMAL_INS;
```
