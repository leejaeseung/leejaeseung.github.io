---
layout: post
title: "한국 정보 올림피아드"
date: 2020-01-10 20:08:10
categories: Algorithm BOJ
image: 'https://leejaeseung.github.io/img/백준로고.png'
---
* * *

# 엘리베이터(2593)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![KOI1-1](https://leejaeseung.github.io/img/KOI/KOI1_1.PNG)
![KOI1-2](https://leejaeseung.github.io/img/KOI/KOI1_2.PNG)
</div>
</details>

* * *

## 풀이

BFS 응용 문제입니다.  
층에 대한 인접 행렬, 엘리베이터에 대한 인접 행렬 총 두개의 인접 행렬을 만들어 줍니다.  

ex)층에 대한 인접 행렬
```
7층 - 1번 엘리베이터, 2번 엘리베이터
```

ex)엘리베이터에 대한 인접 행렬
```
1번 엘리베이터 - 4층, 7층, 10층
```

A와 B , 즉 층에 대한 값을 인수로 받았으니 다음과 같이 BFS로 엘리베이터를 탐색합니다.(방문 여부는 엘리베이터와 층 둘 다 해주었습니다.)
1. 현재 층을 갈 수 있는 엘리베이터를 모두 탐색
2. 탐색한 모든 엘리베이터에 대해 다음으로 갈 층을 큐에 삽입
3. B층을 발견하면 카운트 리턴

경로값은 trace배열로 이전 엘리베이터를 기억하게 하여 역으로 출력합니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.*;

public class Main {

    public static int N,M,A,B;
    public static ArrayList<ArrayList<Integer>> floor;
    public static ArrayList<ArrayList<Integer>> elevator;
    public static boolean[] visit;
    public static boolean[] visit_elv;
    public static int[] trace;
    public static int lastElv = 0;
    
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        floor = new ArrayList<>();
        elevator = new ArrayList<>();
        visit = new boolean[N + 1];
        visit_elv = new boolean[M + 1];
        trace = new int[M + 1];

        for (int i = 0; i <= M ; i++) {
            elevator.add(new ArrayList<>());
        }

        for (int i = 0; i <= N ; i++) {
            floor.add(new ArrayList<>());
        }

        for (int i = 1; i <= M ; i++) {
            st = new StringTokenizer(br.readLine());
            int Xi = Integer.parseInt(st.nextToken());
            int Yi = Integer.parseInt(st.nextToken());
            int temp = 0;

            for (int j = 0; Xi + Yi * j <= N ; j++ ) {
                temp = Xi + Yi * j;
                elevator.get(i).add(temp);
                floor.get(temp).add(i);
            }
        }
        st = new StringTokenizer(br.readLine());
        A = Integer.parseInt(st.nextToken());
        B = Integer.parseInt(st.nextToken());

        bw.write(BFS(A) + "\n");

        Stack<Integer> res = new Stack<>();
        int temp = lastElv;
        while(temp != 0){
            res.push(temp);
            temp = trace[temp];
        }
        while(!res.isEmpty()){
            bw.write(res.pop() + "\n");
        }
        bw.flush();
        bw.close();
    }
    public static int BFS(int start){
        Queue<tuple> q = new LinkedList<>();
        visit[start] = true;
        q.offer(new tuple(start, 0));
        int cnt = 0;

        while(!q.isEmpty()){
            int size = q.size();
            //카운트는 매번 하지않고 엘리베이터가 바뀔때만 해줍니다.
            cnt++;

            for (int j = 0; j < size ; j++) {

                int now_floor = q.peek().floor;
                int now_elv = q.poll().elv;

                for (int elv : floor.get(now_floor)) {
                    if (!visit_elv[elv]) {
                        visit_elv[elv] = true;
                        
                        for (int next_floor : elevator.get(elv)) {
                            if (!visit[next_floor]) {
                                //이전 엘리베이터를 기억합니다.
                                trace[elv] = now_elv;
                                if (next_floor == B) {
                                    lastElv = elv;
                                    return cnt;
                                }
                                
                                visit[next_floor] = true;
                                q.offer(new tuple(next_floor, elv));
                            }
                        }
                    }
                }
            }
        }
        return -1;
    }
}

class tuple{
    int floor;
    int elv;
    public tuple(int floor, int elv){
        this.floor = floor;
        this.elv = elv;
    }
}

```

![KOI1-3](https://leejaeseung.github.io/img/KOI/KOI1_3.PNG)

</div>
</details>

* * *


# 오아시스 재결합(3015)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![KOI1-1](https://leejaeseung.github.io/img/KOI/KOI2_1.PNG)
</div>
</details>

* * *

## 풀이

다음 사람(next)을 스택의 top과 비교해가며 카운트해주는데, 다음과 같은 방식으로 스택을 조정합니다.  

1. next < stack.top --> 카운트만 1 증가시키고, next를 push합니다.(top과 next만 마주보는 경우밖에 없습니다.)  
```

Stack       next
5 4 3 2     1
Stack
5 4 3 2 1
cnt++

```
2. next >= stack.top --> 스택에서 next보다 작거나 같은 값은 모두 pop해주면서 카운트합니다.(next보다 작은 값은 다른 사람과 마주 볼 기회가 더이상 없습니다.즉, 필요없는 값이므로 제거합니다.)  
    1.  next > stack.top 이 될때까지 스택을 pop해주는데, next == stack.top인 경우를 카운트 해줍니다.(키가 같은 사람은 스택에 남아있어야 합니다.)  
    2. 현재 스택은 stack.top보다 키가 작은 사람은 모두 없어진 상태인데, next와 키가 같은 사람들을 모두 스택에 push 해줍니다.(시간 초과 발생)  
```

Stack       next
5 4 3 2     4

2,3,4 제거 cnt += 3 , next와 같은 사람 1명

Stack       next
5           4

cnt++

Stack
5 4 4

```

위 방식대로 하면 키가 같은 사람들이 많을 때 스택에 다시 push하는 부분에서 시간 초과가 납니다.  
따라서 스택에 값을 넣을 때 같은 키라면 사람의 수만 증가시켜 줍니다.(Run-length coding 식으로)  
```

5 5 5 4 4 4 3 3 2 2 2 2

Stack
(5,3),(4,3),(3,2),(2,4)

```


* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.Stack;

public class Main {

    public static void main(String[] argc) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        Stack<tuple> stack = new Stack<>();

        int N = Integer.parseInt(br.readLine());
        stack.push(new tuple(Integer.parseInt(br.readLine()), 1));

        long cnt = 0;
        for (int i = 0; i < N - 1 ; i++) {
            int next = Integer.parseInt(br.readLine());
            tuple temp = null;

            if(stack.peek().tall <= next){
                while(!stack.isEmpty()){
                    if(stack.peek().tall < next){
                        cnt += stack.pop().same_cnt;
                    }
                    else if(stack.peek().tall == next){
                        temp = stack.pop();
                        cnt += temp.same_cnt;
                    }
                    else {
                        cnt++;
                        break;
                    }
                }
            }
            else{
                cnt++;
            }
            if(temp != null){
                temp.same_cnt++;
                stack.push(temp);
            }
            else
                stack.push(new tuple(next, 1));
        }

        bw.write(Long.toString(cnt));
        bw.flush();
        bw.close();
    }
}

class tuple{
    int tall;
    int same_cnt;
    public tuple(int tall, int same_cnt){
        this.tall = tall;
        this.same_cnt = same_cnt;
    }
}

```

![KOI1-3](https://leejaeseung.github.io/img/KOI/KOI2_2.PNG)

</div>
</details>

* * *
