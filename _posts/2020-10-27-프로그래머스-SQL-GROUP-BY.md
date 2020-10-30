---
title: 프로그래머스 SQL 고득점 Kit GROUP BY
date: 2020-10-27 18:00:00 +0800
categories: [Develop, SQL]
tags: [프로그래머스, sql]
---

[프로그래머스 SQL GROUP BY](https://programmers.co.kr/learn/courses/30/parts/17044)  

# 정답

## 1. 고양이와 개는 몇 마리 있을까  
```sql
select ANIMAL_TYPE, count(ANIMAL_TYPE) as count from ANIMAL_INS
group by ANIMAL_TYPE
order by ANIMAL_TYPE asc;
```

## 2. 동명 동물 수 찾기  
```sql
select NAME, count(NAME) as COUNT from ANIMAL_INS 
group by NAME
having NAME != 'NULL' and count(NAME) > 1
order by NAME asc;
```

## 3. 입양 시각 구하기(1)  
```sql
select hour(DATETIME) as HOUR, count(DATETIME) as COUNT
from ANIMAL_OUTS 
group by hour(DATETIME)
having hour >= 9 and hour < 20
order by hour asc;
```

## 4. 입양 시각 구하기(2)  
```sql
set @hour := -1;
select (@hour := @hour+1) as HOUR,
    (select count(*) from ANIMAL_OUTS
    where hour(DATETIME) = @hour) as COUNT
from ANIMAL_OUTS
where @hour < 23;
```
