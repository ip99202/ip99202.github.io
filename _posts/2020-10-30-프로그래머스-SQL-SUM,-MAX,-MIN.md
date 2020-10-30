---
title: 프로그래머스 SQL 고득점 Kit SUM, MAX, MIN
date: 2020-10-26 18:00:00 +0800
categories: [Develop, SQL]
tags: [프로그래머스, sql]
---

[프로그래머스 SQL SUM, MAX, MIN](https://programmers.co.kr/learn/courses/30/parts/17043)  

# 정답

## 1. 최댓값 구하기  
```sql
select DATETIME as 시간 from ANIMAL_INS 
order by DATETIME desc
limit 1;
```
```sql
select max(DATETIME) as 시간 from ANIMAL_INS;
```

## 2. 최솟값 구하기  
```sql
select DATETIME as 시간 from ANIMAL_INS 
order by DATETIME asc
limit 1;
```
```sql
select min(DATETIME) as 시간 from ANIMAL_INS;
```

## 3. 동물 수 구하기  
```sql
select count(ANIMAL_ID) as count from ANIMAL_INS;
```

## 4. 중복 제거하기  
```sql
select count(distinct NAME) as count from ANIMAL_INS
where NAME != 'NULL';
```
