---
layout: post
title: "BOJ 대학교 대회 - 성균관 대학교(4/7)"
date: 2020-01-05 19:17:10
categories: Algorithm BOJ
---
* * *

# Q1 - 교수님 저는 취업할래요(18221)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q1-1](https://leejaeseung.github.io/img/SU/Q1_1.PNG)
![Q1-2](https://leejaeseung.github.io/img/SU/Q1_2.PNG)
</div>
</details>

* * *

## 풀이

성규와 교수님을 꼭지점으로 가지는 직사각형을 그린 뒤, 그 직사각형 내에 성규가 아닌 학생이 3명 이상 존재하면 1을 출력, 아니라면 0을 출력합니다.  
(단, 성규와 교수님의 점과 점 사이의 거리가 5보다 작다면 무조건 0을 출력합니다.)  
  
처음엔 2차원 배열에 맵을 전부 저장하는 방법을 생각했었으나, 불필요한 계산이 많아질 것 같아 성규와 교수님만 따로 변수에 저장하고,   
성규가 아닌 학생의 좌표들을 큐에 집어넣고, 한 명씩 큐에서 빼가며 직사각형 내에 포함되는지 판단하여 카운트 했습니다.  
  
주의해야 할 점은 성규와 교수님의 위치가 직사각형 4개의 꼭지점 중 어느 곳이든 될 수 있기 때문에 코드에서 일반화를 시켜주어야 합니다.  
(min,max 함수를 이용하면 항상 성규와 교수님의 사잇값을 찾을 수 있습니다.)  

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());

        int[][] cl = new int[N][N];
        XY pro = new XY(0,0);
        XY SG = new XY(0,0);
        Queue<XY> q = new LinkedList<>();

        for (int i = 0; i < N ; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N ; j++) {
                cl[i][j] = Integer.parseInt(st.nextToken());
                if(cl[i][j] == 5){
                    pro = new XY(i ,j);
                }
                if(cl[i][j] == 2){
                    SG = new XY(i ,j);                    
                }
                if(cl[i][j] == 1){
                    //성규가 아닌 학생들을 큐에 넣습니다.
                    q.offer(new XY(i, j));
                }
            }
        }
        if(Math.pow(Math.pow(pro.x - SG.x, 2) + Math.pow(pro.y - SG.y, 2), 0.5) < 5 ){    
          //성규와 교수님의 거리가 5보다 작으면 바로 0을 출력합니다.
            System.out.println(0);
        }
        else{
            int cnt = 0;
            while(!q.isEmpty() && cnt < 3){
                XY other = q.poll();
                if(other.x >= Math.min(pro.x, SG.x) && other.x <= Math.max(pro.x, SG.x) && 
                other.y >= Math.min(pro.y, SG.y) && other.y <= Math.max(pro.y, SG.y)){
                  //성규나 교수님이 어느 꼭지점이든 min()은 가장 왼쪽 or 위쪽, max()는 가장 오른쪽 or 아래쪽입니다.
                    cnt++;
                }
            }
            if(cnt >= 3){
                System.out.println(1);
            }
            else{
                System.out.println(0);
            }
        }
    }
}

class XY{
    int x;
    int y;
    public XY(int x, int y){
        this.x = x;
        this.y = y;
    }
}

```

![Q1-3](https://leejaeseung.github.io/img/SU/Q1_3.PNG)

</div>
</details>

* * *


# Q2 - 투에-모스 문자열(18222)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q2-1](https://leejaeseung.github.io/img/SU/Q2_1.PNG)
</div>
</details>

* * *

## 풀이

문제에서 입력값이 10^18 이므로 k번째까지 문자열을 만들게 되면 시간 초과가 뜹니다.  
문자열을 만들어 가기보다, 반대로 k번째부터 가장 처음 문자인 0으로 돌아오는 방법을 생각해봐야 합니다.  
  
문제에서, 새로운 문자열이 오른쪽에 붙습니다. 즉, 왼쪽 문자열은 절대 변하지 않습니다.  
그리고 문자열의 길이는 1부터 시작해 x2 씩 증가합니다.  
이를 이용해 역으로 문자열을 줄여나가며 구할 수 있습니다!  

![Q2-2](https://leejaeseung.github.io/img/SU/Q2_2.PNG)

간단한 예로, k = 7 일 때, 문자열의 총 길이는 8이면 됩니다. 즉, k보다 큰 2의 제곱수 중 최솟값이 처음의 총 길이입니다.  
총 길이를 구한 뒤, 이전 길이(총 길이 / 2)를 k에서 뺍니다.  

![Q2-3](https://leejaeseung.github.io/img/SU/Q2_3.PNG)

위 그림처럼 새로운 총 길이, 이전 길이, k가 구해집니다.그리고 위 작업을 수행할 때마다 카운트를 세줍니다.  
이 작업을 k가 1이 될 때까지 반복합니다.  
k가 1이 됐다면 이제 카운트 값으로 원래 값을 구할 수 있습니다!  
홀수 번 반복해서 0이 됐다는 건 원래 값이 1이었다는 뜻이고, 짝수 번이라면 0이었다는 뜻입니다!  
  
아래 코드에선 처음엔 문자열 번호가 0부터 시작인 줄 알고 k가 0이 되는 것으로 짰습니다..ㅎㅎ  

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        long K = Long.parseLong(br.readLine()) - 1;
            long length = 2;
            if(K == 1) {
              //K가 2일 때 입니다..
                System.out.println(1);
            }
            else if(K == 0 ) {
              //K가 1일 때 입니다..
                System.out.println(0);
            }
            else {
                while (K >= length * 2) {
                    length *= 2;
                }
                int cnt = 0;
                while (true) {
                    K -= length;
                    cnt++;
                    if (K == 0) {
                      //K가 1이 되면 멈춥니다.
                        if (cnt % 2 == 0)
                            System.out.println(0);
                        else
                            System.out.println(1);
                        break;
                    }
                    while (K < length) {
                        length /= 2;
                    }
                }
            }
    }
}

```

![Q2-4](https://leejaeseung.github.io/img/SU/Q2_4.PNG)

</div>
</details>

* * *

# Q3 - 민준이와 마산 그리고 건우(18223)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q3-1](https://leejaeseung.github.io/img/SU/Q3_1.PNG)
![Q3-2](https://leejaeseung.github.io/img/SU/Q3_2.PNG)
![Q3-3](https://leejaeseung.github.io/img/SU/Q3_3.PNG)
</div>
</details>

* * *

## 풀이

최단 거리를 구하는 문제이므로 다익스트라 알고리즘을 사용하여 풀이했습니다.  
단, 건우를 데리고 가야 하므로 민준->도착지 와 건우->도착지 두 개의 거리를 모두 구해야 합니다.  
민준->건우의 길이 + 건우->도착지의 길이  <=  민준->도착지의 길이  == "SAVE HIM"  
민준->건우의 길이 + 건우->도착지의 길이  >  민준->도착지의 길이  == "GOOD BYE"  

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {


    public static ArrayList<ArrayList<tuple>> dis;
    public static int V,E;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());
        int P = Integer.parseInt(st.nextToken());

        dis = new ArrayList<>();
        for (int i = 0; i <= V ; i++) {
            dis.add(new ArrayList<>());
        }


        for (int i = 0; i < E ; i++) {
            st = new StringTokenizer(br.readLine());
            int from = Integer.parseInt(st.nextToken());
            int to = Integer.parseInt(st.nextToken());
            int distance = Integer.parseInt(st.nextToken());
            dis.get(from).add(new tuple(distance, to));
            dis.get(to).add(new tuple(distance, from));
        }

        int[] MGdis = Dijkstra(1);
        int[] GWdis = Dijkstra(P);
        //민준, 건우의 도착지에 대한 최단 거리를 모두 구합니다.

        if(MGdis[V] >= MGdis[P] + GWdis[V]){
            //거리를 비교하여 건우를 데리고 갈 수 있는지 정합니다.
            System.out.println("SAVE HIM");
        }
        else{
            System.out.println("GOOD BYE");
        }
    }
    public static int[] Dijkstra(int start){
        PriorityQueue<tuple> pq = new PriorityQueue<>(new Comparator<tuple>() {
            @Override
            public int compare(tuple o1, tuple o2) {
                return o1.dis >= o2.dis ? 1 : -1;
            }
        });

        pq.offer(new tuple(0 , start));
        int[] distance = new int[V + 1];
        for (int i = 1; i <= V ; i++) {
            distance[i] = 55000000;
        }

        distance[start] = 0;

        while(!pq.isEmpty()){
            tuple now = pq.poll();

            if(distance[now.node] < now.dis)
                continue;

            for (int i = 0; i < dis.get(now.node).size() ; i++) {
                tuple next = dis.get(now.node).get(i);
                if(distance[next.node] > distance[now.node] + next.dis){
                    distance[next.node] = distance[now.node] + next.dis;
                    pq.offer(new tuple(distance[next.node], next.node));
                }
            }
        }
        return distance;
    }
}

class tuple{
    int dis;
    int node;
    public tuple(int dis, int node){
        this.dis = dis;
        this.node = node;
    }
}


```

![Q3-4](https://leejaeseung.github.io/img/SU/Q3_4.PNG)

</div>
</details>

* * *

# Q4 - 미로에 갇힌 건우(18224)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q4-1](https://leejaeseung.github.io/img/SU/Q4_1.PNG)
![Q4-2](https://leejaeseung.github.io/img/SU/Q4_2.PNG)
![Q4-3](https://leejaeseung.github.io/img/SU/Q4_3.PNG)
</div>
</details>

* * *

## 풀이

BFS 응용 문제입니다.  
낮과 밤에 따라 벽을 넘을 수 있는지가 달라지므로 visit[x][y]만으론 아래와 같은 예제를 처리할 수 없습니다.  
```

5 5
0 1 0 0 0
0 1 0 0 0
0 1 0 0 0
0 1 0 0 0
0 1 0 0 0

```
위 예제에서 (0,4) 까지 걸어도 낮이므로 벽을 넘지 못하고, 이전 블록은 이미 방문했으므로 끝나게 됩니다.  
따라서, 기존 BFS의 visit 배열에 두 가지 속성을 더 추가해 줍니다.  
visit[x][y][걸음 수][낮 or 밤]  
  
도착지에 도착했을 때의 총 걸음 수로 몇 번째 날인지, 낮인지 밤인지 계산하여 출력해줍니다.  

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {

    public static int N,M;
    public static int[][] map;
    public static boolean[][][][] visit;        //x,y,걸음 수, 낮 or 밤
    public static int[] mv1 = {0,0,1,-1};
    public static int[] mv2 = {1,-1,0,0};
    public static int ans_step = -1;
    public static int ans_isSun = 1;

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        map = new int[N][N];
        visit = new boolean[N][N][M + 1][2];

        for (int i = 0; i < N ; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N ; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        BFS();

        if(ans_step != -1) {
            bw.write((ans_step / (M * 2) + 1 )  + " ");
            //몇 번째 날인지 계산합니다.
            if (ans_isSun == 1)
                bw.write("sun");
            else
                bw.write("moon");
        }
        else
            bw.write(Integer.toString(-1));
        bw.flush();
        bw.close();
    }
    public static void BFS(){
        Queue<XY> q = new LinkedList<>();
        q.offer(new XY(0,0, 0, 1));
        visit[0][0][0][1] = true;

        while(!q.isEmpty()){
            XY now = q.poll();

            if(now.x == N - 1 && now.y == N - 1){
                ans_step = now.step;
                ans_isSun = now.isSun;
                return;
            }

            for (int i = 0; i < 4; i++) {
                int next_x = now.x + mv1[i];
                int next_y = now.y + mv2[i];
                int next_step = now.step + 1;       //걸음 수는 항상 1씩 증가합니다.(총 걸음 수)
                int next_isSun = now.isSun;

                if(next_step % M == 0)              //낮과 밤이 바뀌는 걸 계산합니다.
                    next_isSun = (next_isSun + 1) % 2;

                if (next_x < 0 || next_y < 0 || next_x >= N || next_y >= N) continue;
                if(now.isSun == 1 && map[next_x][next_y] == 1)   continue;
                if(now.isSun == 0 && map[next_x][next_y] == 1){
                    //벽을 넘어가는 부분입니다.
                    while((next_x >= 0 && next_y >= 0 && next_x < N && next_y < N) && map[next_x][next_y] == 1 ){
                        next_x += mv1[i];
                        next_y += mv2[i];
                    }
                    if(next_x < 0 || next_y < 0 || next_x >= N || next_y >= N)  continue;
                }
                if(!visit[next_x][next_y][next_step % M][next_isSun]){
                    visit[next_x][next_y][next_step % M][next_isSun] = true;
                    //방문을 확인할 땐 걸음 수 % M 으로 구합니다.

                    q.offer(new XY(next_x, next_y, next_step, next_isSun));
                }
            }
        }
    }
}

class XY{
    int x;
    int y;
    int step;
    int isSun;
    public XY(int x, int y, int step, int isSun){
        this.x = x;
        this.y = y;
        this.step = step;
        this.isSun = isSun;
    }
}

```

![Q4-4](https://leejaeseung.github.io/img/SU/Q4_4.PNG)

</div>
</details>