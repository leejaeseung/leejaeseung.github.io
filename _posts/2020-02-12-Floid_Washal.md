---
layout: post
title: "플로이드-와샬 알고리즘"
date: 2020-02-12 20:08:10
categories: Algorithm
---

# 플로이드 와샬 알고리즘이란?

점과 점 사이의 최단 거리를 구하는 알고리즘으로 **모든 점에서부터 모든 점까지의 최단 거리**를 구할 수 있습니다.  
또한 DP 를 이용한 알고리즘이기 때문에 **음수 가중치 처리**가 가능합니다.(그리디 알고리즘인 다익스트라 알고리즘은 불가능합니다.)  
기본적인 아이디어는 다음과 같습니다.

```
a = 시작점
b = 도착점
c = 거쳐가는 노드
a->b 로의 최단 거리는 a->b 와 a->c->b 중 작은 값
모든 점 c 에 대해 위 작업을 수행하면 모든 점에서의 모든 점까지의 최단 거리를 구할 수 있다.
```

즉, a 에서 b 로 갈 떄 다른 점(c)을 *거쳐가거나* 아니면 그대로 갈 지 선택하는 것입니다.

이론에 비해 매우 간단하게 3중 반복문으로 구현할 수 있습니다.

```

public static void Floid(){
        for (int i = 1; i <= N ; i++) {
            for (int j = 1; j <= N ; j++) {
                for (int k = 1; k <= N ; k++) {
                    dist[j][k] = Math.min(dist[j][k], dist[j][i] + dist[i][k]);
                    //dist 배열에서 자기 자신으로 가는 연결은 0, 다른 값들은 최대값(무한)으로 초기화합니다.
                }
            }
        }
    }

```

위 코드에서 dist[j][k] 는 그대로 가는 경로고, dist[j][i] + dist[i][k] 는 i 노드를 거쳐가는 경로입니다.  
플로이드 와샬 알고리즘은 O(N^3)의 시간 복잡도로, 최단 거리 알고리즘 중 가장 느립니다.  
또한 모든 점 -> 모든 점이기 때문에 공간 복잡도는 O(N^2)이 됩니다.

- 장점
1. 음수 가중치를 처리할 수 있다.(사이클이 없다면)
2. 모든 점에서의 모든 점까지의 최단 거리를 한번에 구할 수 있다.
3. 다른 최단 거리 알고리즘에 비해 구현이 쉽다.

- 단점
1. O(N^3)의 시간 복잡도로 가장 느리다.

# 연습 문제

* * *

# 엘리베이터(2593)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![KOI1-1](https://leejaeseung.github.io/img/FW/FW1_1.PNG)
</div>
</details>

* * *

## 풀이

플로이드 와샬 기본 문제입니다.  
모든 최단 경로를 구하고, 집->X 의 거리 + X->집 의 거리가 최대인 학생을 구합니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static int N, M, X;
    public static int[][] dist;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        X = Integer.parseInt(st.nextToken());

        dist = new int[N + 1][N + 1];
        for (int i = 0; i <= N ; i++) {
            for (int j = 0; j <= N ; j++) {
                dist[i][j] = 10000000;
                if(i == j)
                    dist[i][j] = 0;
                //자기 자신은 0으로 초기화!
            }
        }

        for (int i = 0; i < M ; i++) {
            st = new StringTokenizer(br.readLine());

            int from = Integer.parseInt(st.nextToken());
            int to = Integer.parseInt(st.nextToken());
            int d = Integer.parseInt(st.nextToken());

            dist[from][to] = d;
        }
        Floid();

        int max = 0;
        for (int i = 1; i <= N ; i++) {
            max = Math.max(max, dist[i][X] + dist[X][i]);
        }

        bw.write(Integer.toString(max));
        bw.flush();
        bw.close();
    }
    public static void Floid(){
        for (int i = 1; i <= N ; i++) {
            for (int j = 1; j <= N ; j++) {
                for (int k = 1; k <= N ; k++) {
                    dist[j][k] = Math.min(dist[j][k], dist[j][i] + dist[i][k]);
                    //거리 덧셈에서 오버플로우 조심!
                }
            }
        }
    }
}

```

![KOI1-3](https://leejaeseung.github.io/img/FW/FW1_2.PNG)

</div>
</details>

* * *
