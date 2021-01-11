---
title: 백준 18881 Social Distancing II java
date: 2021-01-11 18:00:00 +0800
categories: [Algorithm, BOJ]
tags: [boj]
---

## 문제
[https://www.acmicpc.net/problem/18881](https://www.acmicpc.net/problem/18881)  
<br>

## 문제해설  
소들 사이에서 COWVID-19 전염병이 돌고 있다.  
감염된 소와의 거리가 R 이하라면 전염이 된다.  
소들의 위치와 감염 여부가 주어지고  
이 정보를 토대로 처음에 감염된 소가 몇마리인지 구하면 된다.  
<br>

## 문제풀이  
일단 소의 위치를 기준으로 오름차순으로 정렬을 시키고  
전체 소 사이의 거리를 반복문을 통해 구하면서 R값을 구한다.  
두 소가 모두 전염병에 걸리지 않았을 경우만 건너뛰면서 R값을 최솟값으로 갱신해주면 된다.  

그 후에 다시 소 사이의 거리를 반복문을 통해 돌면서  
소 사이의 거리가 R보다 큰 경우 새로운 그룹이라고 판별해서 결과값을 증가시켜준다.  
<br>


## 전체코드  
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.StringTokenizer;

class Pair {
    int x, s;

    public Pair(int x, int s) {
        this.x = x;
        this.s = s;
    }
}

public class Main {

    public static int n;
    public static ArrayList<Pair> arr = new ArrayList<>();

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        n = Integer.parseInt(st.nextToken());
        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            arr.add(new Pair(Integer.parseInt(st.nextToken()), Integer.parseInt(st.nextToken())));
        }
        arr.sort(new Comparator<Pair>() {
            @Override
            public int compare(Pair o1, Pair o2) {
                if (o1.x > o2.x)
                    return 1;
                else return -1;
            }
        });

        int R = Integer.MAX_VALUE;
        for (int i = 1; i < n; i++) {
            Pair now = arr.get(i - 1);
            Pair next = arr.get(i);
            if (now.s == 0 || next.s == 0) {
                if (now.s == 0 && next.s == 0)
                    continue;
                R = Math.min(next.x - now.x, R);
            }
        }
        R--;

        int res = 1;
        for (int i = 1; i < n; i++) {
            Pair now = arr.get(i - 1);
            Pair next = arr.get(i);
            if (next.x - now.x > R && next.s == 1) {
                res++;
            }
        }
        System.out.println(res);
        br.close();
    }
}
```