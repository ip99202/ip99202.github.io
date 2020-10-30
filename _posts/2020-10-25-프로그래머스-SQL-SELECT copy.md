---
title: 프로그래머스 SQL 고득점 Kit SELECT
date: 2020-10-25 18:00:00 +0800
categories: [Develop, SQL]
tags: [프로그래머스, sql]
---

[프로그래머스 SQL SELECT](https://programmers.co.kr/learn/courses/30/parts/17042)  

# 정답

## 1. 모든 레코드 조회하기  
```sql
select * from ANIMAL_INS
order by ANIMAL_ID asc;
```

## 2. 역순 정렬하기  
```sql
select NAME, DATETIME from ANIMAL_INS 
order by ANIMAL_ID desc;
```

## 3. 아픈 동물 찾기  
```sql
select ANIMAL_ID, NAME from ANIMAL_INS
where INTAKE_CONDITION = 'Sick'
order by ANIMAL_ID asc;
```

## 4. 어린 동물 찾기  
```sql
select ANIMAL_ID, NAME from ANIMAL_INS 
where INTAKE_CONDITION != 'Aged'
order by ANIMAL_ID asc;
```

## 5. 동물의 아이디와 이름  
```sql
select ANIMAL_ID, NAME from ANIMAL_INS 
order by ANIMAL_ID asc;
```

## 6. 여러 기준으로 정렬하기  
```sql
select ANIMAL_ID, NAME, DATETIME from ANIMAL_INS 
order by NAME, DATETIME desc;
```

## 7. 상위 n개 레코드  
```sql
select NAME from ANIMAL_INS 
order by DATETIME asc
limit 1;
```
