---
layout: post
title: "BOJ 대학교 대회 - 한양 대학교 OPC(14/17)"
date: 2020-01-10 20:08:10
categories: Algorithm BOJ
image: 'https://leejaeseung.github.io/img/백준로고.png'
---
* * *

# Q1 - 과제는 끝나지 않아!(17952)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q1-1](https://leejaeseung.github.io/img/HU/Q1_1.PNG)
![Q1-2](https://leejaeseung.github.io/img/HU/Q1_2.PNG)
</div>
</details>

* * *

## 풀이

prev 변수로 가장 오래전에 풀고 있었던(완료하지 못한) 문제를 기억하면 쉽게 풀 수 있는 문제입니다.  
새로운 문제가 나올 때마다 now 변수는 가장 최근의 문제를 가르키고, 새로운 문제가 나오기 전에  
다 풀었다면 prev 의 문제를 풀고, prev도 다 풀었다면 다음으로 못 푼 문제를 가르키게 합니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());

        int sum = 0;
        int prev = 0;
        int now = -1;
        ArrayList<Assignment> list = new ArrayList<>();
        for (int i = 0; i < N ; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int exist = Integer.parseInt(st.nextToken());
            if(exist == 1){
                now = list.size() - 1;
                int score = Integer.parseInt(st.nextToken());
                int time = Integer.parseInt(st.nextToken());
                list.add(new Assignment(score, time));
                now++;
            }
            if(now != -1) {
                list.get(now).remainTime--;

                if (list.get(now).remainTime == 0) {
                    sum += list.get(now).score;
                    prev = now;
                    while (list.get(prev).remainTime == 0) {
                        prev--;
                        if (prev < 0) {
                            break;
                        }
                    }
                    if (prev != -1)
                        now = prev;
                }
            }
        }
        System.out.print(sum);
    }
}

class Assignment{

    int score;
    int remainTime;
    public Assignment(int score, int remainTime){
        this.score = score;
        this.remainTime = remainTime;
    }
}

```

![Q1-3](https://leejaeseung.github.io/img/HU/Q1_3.PNG)

</div>
</details>

* * *


# Q2 - 흩날리는 시험지 속에서 내 평점이 느껴진거야(17951)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q2-1](https://leejaeseung.github.io/img/HU/Q2_1.PNG)
![Q2-2](https://leejaeseung.github.io/img/HU/Q2_2.PNG)
</div>
</details>

* * *

## 풀이

이분 탐색의 한 종류인 parametric search 문제입니다.  
각 시험지의 점수를 이분 탐색하는 것이 아닌 찾고자 하는 결과값인 최대 점수(예제 입력 1에선 0 ~ 108)  
에 대해 이분 탐색을 해야 합니다.

예제 입력1  

```
8 2
12 7 19 20 17 14 9 10
```

즉, 0 ~ 108 의 중간 값인 54(mid)에서 com() 함수를 이용해 찾고자 하는 값이 맞는지 판단합니다.  
시험지 그룹은 연속적이므로 처음부터 더해가며 54(mid)를 넘어가면 그룹을 추가해주고,  
총 만들어진 그룹의 수가 K 보다 크다면 -> 그룹을 줄여줘야 합니다. -> false return -> mid를 증가시켜야 합니다.  
-> left = mid + 1;  
총 만들어진 그룹의 수가 K 보다 작거나 같다면 -> 그룹을 늘리거나, 최소값을 찾아야 합니다. -> true return -> mid를 감소시켜야 합니다.  
-> right = mid - 1;  

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static int[] value;
    public static int N,K;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());

        value = new int[N];

        st = new StringTokenizer(br.readLine());
        int sum = 0;

        for (int i = 0; i < N ; i++) {
            value[i] = Integer.parseInt(st.nextToken());
            sum += value[i];
        }

        int left = 0;
        int right = sum;
        int res = 0;
        while (left <= right){
            int mid = (left + right) / 2;

            if(com(mid)){
                //mid 를 감소
                res = mid;
                right = mid - 1;
            }
            else{
                //mid 를 증가
                left = mid + 1;
            }
        }
        System.out.println(res);
    }
    public static boolean com(int mid){

        int group = 1;
        int sum = 0;

        for (int i = 0; i < N ; i++) {
            if(mid < value[i])
                return false;

            sum += value[i];
            if(sum > mid){
                sum = 0;
                group++;
                //그룹 추가
            }
        }
        return group <= K;
    }
}

```

![Q2-3](https://leejaeseung.github.io/img/HU/Q2_3.PNG)

</div>
</details>

* * *


# Q3 - Drop The Byte!(17949)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q3-1](https://leejaeseung.github.io/img/HU/Q3_1.PNG)
![Q3-2](https://leejaeseung.github.io/img/HU/Q3_2.PNG)
</div>
</details>

* * *

## 풀이

java에서 제공하는 Long.parseLong() 을 이용해 풀이했습니다.
위 메서드는 첫 번째 인자로 문자열을 받아 long형 값으로 바꿔주는 메서드입니다.
두 번째 인자를 넣게 되면 첫 번째 인자인 문자열을 두 번째 인자의 형식(n진수)으로 읽어들여 출력합니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String s = br.readLine();
        int N = Integer.parseInt(br.readLine());
        StringTokenizer st = new StringTokenizer(br.readLine());
        int point = 0;

        for (int i = 0; i < N ; i++) {
            String form = st.nextToken();
            if(form.equals("char")){
                //char는 두 자리만 읽습니다.
                bw.write(Long.parseLong(s.substring(point, point = point + 2), 16) + " ");
            }
            else if(form.equals("int")){
                //char는 여덟 자리만 읽습니다.
                bw.write(Long.parseLong(s.substring(point, point = point + 8), 16) + " ");
            }
            else if(form.equals("long_long")){
                //char는 열여섯 자리만 읽습니다.
                bw.write(Long.parseLong(s.substring(point, point = point + 16), 16) + " ");
            }
        }
        bw.flush();
        bw.close();
    }
}

```

![Q3-3](https://leejaeseung.github.io/img/HU/Q3_3.PNG)

</div>
</details>

* * *


# Q4 - 퐁당퐁당 1(17944)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q4-1](https://leejaeseung.github.io/img/HU/Q4_1.PNG)
![Q4-2](https://leejaeseung.github.io/img/HU/Q4_2.PNG)
</div>
</details>

* * *

## 풀이

게임의 규칙대로 반복문을 이용해 풀었습니다.  
팔의 개수가 1 아래로 가게 되면 팔의 개수를 더해 주고, 2 x N 을 넘어가면 팔의 개수를 감소시킵니다. 

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int T = Integer.parseInt(st.nextToken());

        int arm = 1;
        int cnt = 1;
        int p = -1;

        while(cnt != T){
            if(arm == 2 * N || arm == 1)
            //팔의 개수를 더하거나 빼줍니다.
                p *= -1;
            arm += p;
            cnt++;
        }
        System.out.println(arm);
    }
}

```

![Q4-3](https://leejaeseung.github.io/img/HU/Q4_3.PNG)

</div>
</details>

* * *


# Q5 - 뜨끈한 돼지국밥(17948)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
</div>
</details>

* * *

## 풀이

x

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">
</div>
</details>

* * *


# Q6 - 피자는 나눌 수록 커지잖아요(17946)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q6-1](https://leejaeseung.github.io/img/HU/Q6_1.PNG)
![Q6-2](https://leejaeseung.github.io/img/HU/Q6_2.PNG)
</div>
</details>

* * *

## 풀이

피자 자르기 공식에 의해 n번 칼질 당  
총 피자 수 = (n^2 + n + 2) / 2
줘야하는 피자 수 = (n^2 + n) / 2
둘의 차이이므로, 항상 1 이 됩니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int T = Integer.parseInt(br.readLine());
        for (int i = 0; i < T ; i++) {
            int K = Integer.parseInt(br.readLine());
            bw.write(1 + "\n");
        }
        bw.flush();
        bw.close();
    }
}

```

![Q6-3](https://leejaeseung.github.io/img/HU/Q6_3.PNG)

</div>
</details>

* * *


# Q7 - 투튜브(17954)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q7-1](https://leejaeseung.github.io/img/HU/Q7_1.PNG)
![Q7-2](https://leejaeseung.github.io/img/HU/Q7_2.PNG)
</div>
</details>

* * *

## 풀이

N = 5
7 6 5 4 8
9 3 2 1 10

부패값이 최소가 되려면 최대한 큰 크기를 뽑아내야 합니다.
다음 규칙들을 고려하여 풀이합니다.
1. 위에서(큰 쪽) 4번째까지의 수는 가장 바깥에 존재합니다.(4번째로 큰 수를 뽑아내게 하기 위해)
2. 4번째로 큰 수의 옆에는 N - 2개의 수들이 1씩 감소하며 존재합니다.(3번째로 큰 수까진 뽑을 수가 없기 때문에 차선책으로 바로 아래 크기를 뽑습니다.
3. 2번을 수행해 모두 뽑았다면, 그 줄 마지막엔 세 번째로 큰 수가 존재합니다.(한 줄이 모두 없어집니다.)
4. 나머지 줄은 두 번째로 큰 수와 가장 큰 수가 둘러싸고 있습니다.(다른 줄을 모두 없애기 전까지 뽑지 못합니다.)
5. 두 번째로 큰 수 옆에는 남은 수들 중 가장 큰 수부터 1씩 감소하며 존재합니다.

위 규칙에 따라 배열을 만들어놓고, 시간마다 계산해가며 풀이했습니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;

public class Main {

    public static int N;
    public static int[][] tube;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        N = Integer.parseInt(br.readLine());
        tube = new int[2][N];

        tube[0][0] = 2 * N - 3;
        tube[0][N - 1] = 2 * N - 2;
        tube[1][0] = 2 * N - 1;
        tube[1][N - 1] = 2 * N;
        //가장자리 배치
        for (int i = 1; i < N - 1 ; i++) {
            tube[0][i] = 2 * N - 3 - i;
        }
        int init = 1;
        for (int i = N - 2; i > 0 ; i--) {
            tube[1][i] = init++;
        }

        long res = 0;
        long time = 0;
        long sub = 0;
        if(N > 1) {
            for (int i = 0; i < N - 1; i++) {
                res += (6 * N - 3) * time;
                res += (((2 * N - 3) * (N - 1)) - sub) * time;
                sub += tube[0][i];
                time++;
            }
            res += (6 * N - 3) * time;
            res += (((N - 2) * (N - 1)) / 2) * time;
            time++;
            res += (4 * N - 1) * time;
            res += (((N - 2) * (N - 1)) / 2) * time;
            time++;
            sub = 0;

            for (int i = 0; i < N - 2; i++) {
                res += ((((N - 2) * (N - 1)) / 2) - sub) * time;
                res += (2 * N) * time;
                sub += tube[1][i + 1];
                time++;
            }
            res += 2 * N * time;

            bw.write(res + "\n");
            for (int i = 0; i < 2; i++) {
                for (int j = 0; j < N; j++) {
                    bw.write(tube[i][j] + " ");
                }
                bw.write("\n");
            }
        }
        else{
            bw.write(2 + "\n");
            bw.write(1 + "\n");
            bw.write(2 + "\n");
        }
        bw.flush();
        bw.close();
    }
}

```

![Q7-3](https://leejaeseung.github.io/img/HU/Q7_3.PNG)

</div>
</details>

* * *


# Q8 - 상남자 곽철용(17947)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q8-1](https://leejaeseung.github.io/img/HU/Q8_1.PNG)
</div>
</details>

* * *

## 풀이

?

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.*;

public class Main {

    public static int N,M,K;
    public static int[] cards;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());

        cards = new int[N * 4 + 1];
        cards[0] = -1;

        for (int i = 1; i <= N * 4 ; i++) {
            cards[i] = i % K;
        }

        for (int i = 0; i < M ; i++) {
            st = new StringTokenizer(br.readLine());
            cards[Integer.parseInt(st.nextToken())] = -1;
            cards[Integer.parseInt(st.nextToken())] = -1;
        }

        st = new StringTokenizer(br.readLine());
        int gwack_a = Integer.parseInt(st.nextToken());
        int gwack_b = Integer.parseInt(st.nextToken());
        cards[gwack_a] = -1;
        cards[gwack_b] = -1;

        int gwack = Math.abs(gwack_a % K - gwack_b % K);

        Arrays.sort(cards);
        int highp = cards.length - 1;
        int lowp = cards.length - 2;
        int cnt = 0;

        while(true){
            if(cnt > M - 2){
                break;
            }
            if(lowp <= 2 * M + 2){
                break;
            }
            if(cards[highp] <= gwack)
                break;
            if(Math.abs(cards[lowp] - cards[highp]) > gwack){
                    cnt++;
                    highp--;
                    lowp--;
                }
            else{
                lowp--;
            }
        }

        bw.write(Integer.toString(cnt));
        bw.flush();
        bw.close();
    }
}

```

![Q8-2](https://leejaeseung.github.io/img/HU/Q8_2.PNG)

</div>
</details>

* * *


# Q9 - 통학의 신(17945)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q9-1](https://leejaeseung.github.io/img/HU/Q9_1.PNG)
![Q9-2](https://leejaeseung.github.io/img/HU/Q9_2.PNG)
</div>
</details>

* * *

## 풀이

근의 공식을 사용하여 풀이했습니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        double A = Double.parseDouble(st.nextToken());
        double B = Double.parseDouble(st.nextToken());
        double D = 4 * A * A - 4 * B;

        int x1 = (int)((A * -2 + (int)Math.sqrt(D)) / 2);
        int x2 = (int)((A * -2 - (int)Math.sqrt(D)) / 2);
        if(x1 == x2){
            System.out.println(x1);
        }
        else if(x1 < x2){
            System.out.println(x1 + " " + x2);
        }
        else{
            System.out.println(x2 + " " + x1);
        }
    }
}

```

![Q9-3](https://leejaeseung.github.io/img/HU/Q9_3.PNG)

</div>
</details>

* * *


# Q10 - 스노우볼(17950)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q10-1](https://leejaeseung.github.io/img/HU/Q10_1.PNG)
</div>
</details>

* * *

## 풀이

단순히 x를 곱해가며 합을 더해주고, 더해줄 때 모든 연산에 모듈러 연산의 성질을 이용해   
오버플로우를 방지하며 풀이했습니다.(O(n) 의 시간복잡도)

모듈러 연산의 성질
```
(A + B) % C = ((A % C) + (B % C)) % C
(A - B) % C = ((A % C) - (B % C)) % C
(A * B) % C = ((A % C) * (B % C)) % C
```

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main10 {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        int H = Integer.parseInt(st.nextToken());
        int x = Integer.parseInt(st.nextToken());
        int div = 1000000007;

        long mul = x;
        long sum = 0;

        for (int i = 1; i <= H ; i++) {
            int cnt = Integer.parseInt(br.readLine());
            //모듈러 연산
            sum = ((sum % div) + (((mul % div) * (cnt % div)) % div) % div) % div;
            mul = ((mul % div) * (x % div)) % div;
        }
        System.out.println(sum);
    }
}

```

![Q10-2](https://leejaeseung.github.io/img/HU/Q10_2.PNG)

</div>
</details>

* * *


# Q11 - 디저트(17953)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q11-1](https://leejaeseung.github.io/img/HU/Q11_1.PNG)
![Q11-2](https://leejaeseung.github.io/img/HU/Q11_2.PNG)
</div>
</details>

* * *

## 풀이

DP(다이나믹 프로그래밍) 문제였습니다.
dp[날짜][디저트] 배열을 만들어 각 날짜마다 해당 디저트를 먹었을 때의 최대값을 갱신해줍니다.  
dp[0]은 첫 날이므로 어느 디저트를 먹어도 최대입니다.(그대로 초기화 해줍니다.)
dp[n]은 (dp[n-1](이전 날)의 디저트 만족도) + (오늘의 디저트 만족도) 가 최대가 되는 값을 저장합니다.  
전 날 같은 디저트를 먹었다면 1/2 해주는 것도 고려합니다. 

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main11 {

    public static int[][] satis;
    public static int[][] dp;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        satis = new int[M][N];
        dp = new int[N][M];

        for (int i = 0; i < M ; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N ; j++) {
                satis[i][j] = Integer.parseInt(st.nextToken());
                if(j == 0)
                    dp[j][i] = satis[i][j];
            }
        }
        for (int i = 1; i < N ; i++) {
            for (int j = 0; j < M ; j++) {
                for (int k = 0; k < M ; k++) {
                    if(j != k)
                        dp[i][j] = Math.max(dp[i - 1][k] + satis[j][i], dp[i][j]);
                    else
                        dp[i][j] = Math.max(dp[i - 1][k] + (satis[j][i] / 2), dp[i][j]);
                        //전 날 같은 것을 먹는다면( j == k -> 디저트가 같다.)
                }
            }
        }
        int res = 0;
        for (int i = 0; i < M ; i++) {
            res = Math.max(res, dp[N - 1][i]);
        }
        System.out.println(res);
    }
}

```

![Q11-3](https://leejaeseung.github.io/img/HU/Q11_3.PNG)

</div>
</details>

* * *


# Q12 - 퐁당퐁당 2(17938)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q12-1](https://leejaeseung.github.io/img/HU/Q12_1.PNG)
![Q12-2](https://leejaeseung.github.io/img/HU/Q12_2.PNG)
</div>
</details>

* * *

## 풀이

풍당퐁당1 의 코드를 재사용하여 풀이했습니다.  
팔의 총 개수가 한 바퀴(2 * N)을 돌 때마다 모듈러 연산을 해줍니다.  
가장 마지막 이전의 팔 번호(left)와 가장 마지막 팔 번호(right) 사이에 휘수의 위치가 있는지 판단합니다.  
이 때, left와 right의 사잇값을 찾을 때 주의합니다.

left > right
```
{x,right,x,x,x,x,x,x,x,left,x}
```
left <> right
```
{x,left,x,x,x,x,x,x,x,right,x}
```

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main12 {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        StringTokenizer st = new StringTokenizer(br.readLine());
        int P = Integer.parseInt(st.nextToken());
        int T = Integer.parseInt(st.nextToken());

        int arm = 1;
        int cnt = 1;
        int p = -1;
        int sum = 0;
        int left = 0;
        int right = 0;

        while(cnt != T){
            if(arm == 2 * N || arm == 1)
                p *= -1;
            arm += p;
            if(cnt == T -1) {
                left = sum;
            }
            sum = (sum + arm) % (N * 2);
            if(cnt == T -1) {
                right = sum;
            }
            cnt++;
        }
        boolean flag = true;
        if(left > right){
            //{x,right,x,x,x,x,x,x,x,left,x} 인 경우
            if(left < 2 * P || right >= 2 * P - 1)
                flag = true;
            else
                flag = false;
        }
        else{
            //{x,left,x,x,x,x,x,x,x,right,x} 인 경우
            if(left < 2 * P && right >= 2 * P - 1)
                flag = true;
            else
                flag = false;
        }

        if(flag)
            System.out.println("Dehet YeonJwaJe ^~^");
        else
            System.out.println("Hing...NoJam");
    }
}

```

![Q12-3](https://leejaeseung.github.io/img/HU/Q12_3.PNG)

</div>
</details>

* * *


# Q13 - 목장 CCTV(17941)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
</div>
</details>

* * *

## 풀이

Hard

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">
</div>
</details>

* * *


# Q14 - 알고리즘 공부(17942)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q14-1](https://leejaeseung.github.io/img/HU/Q14_1.PNG)
![Q14-2](https://leejaeseung.github.io/img/HU/Q14_2.PNG)
</div>
</details>

* * *

## 풀이

푸는중

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">
</div>
</details>

* * *


# Q15 - Gazzzua(17939)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q15-1](https://leejaeseung.github.io/img/HU/Q15_1.PNG)
![Q15-2](https://leejaeseung.github.io/img/HU/Q15_2.PNG)
</div>
</details>

* * *

## 풀이

우선순위 큐에 최대 가격과 그 시간을 집어넣습니다.(가격이 같다면 빠른 시간 순서로 넣어야 합니다.)  
최대 가격을 발견하면 현재까지의 코인을 모두 팔고 다음 최대값으로 갱신합니다.  
최대값을 갱신할 때, 이미 지난 시간이라면 제외시켜야 합니다.  
그리고 마지막까지 최대값이 나오지 않았다면(코인을 팔지 못했다면) 다시 같은 가격에 되팔아야 합니다.  

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        int[] price = new int[N];

        PriorityQueue<tuple> pq = new PriorityQueue<>(new Comparator<tuple>() {
            @Override
            public int compare(tuple o1, tuple o2) {
                if(o1.price < o2.price)
                    return 1;
                else if( o1.price > o2.price)
                    return -1;
                else{
                    //가격이 같다면 빠른 시간 순서로 정렬합니다.
                    return o1.minute >= o2.minute ? 1 : -1;
                }
            }
        });

        StringTokenizer st = new StringTokenizer(br.readLine());

        for (int i = 0; i < N ; i++) {
            price[i] = Integer.parseInt(st.nextToken());
            pq.offer(new tuple(i, price[i]));
        }

        int now = 0;
        tuple nowMax = pq.poll();

        long sum = 0;
        int coin = 0;

        for (int i = 0; i < N ; i++) {
            now = price[i];
            coin++;
            sum -= now;

            if (now == nowMax.price) {
                sum += coin * now;
                coin = 0;
                while(!pq.isEmpty() && pq.peek().minute < i){
                    //이미 지난 시간을 제외합니다.
                    pq.poll();
                }
                if(!pq.isEmpty())
                    nowMax = pq.poll();
                    //최대값을 갱신합니다.
            }
        }
        for (int i = N - 1; i >= N - coin  ; i--) {
            //마지막에 최대값이 나오지 않았다면 다시 그대로 더해줍니다.
            sum += price[i];
        }
        System.out.println(sum);
    }
}

class tuple{
    int minute;
    int price;
    public tuple(int minute, int price){
        this.minute = minute;
        this.price = price;
    }
}

```

![Q15-3](https://leejaeseung.github.io/img/HU/Q15_3.PNG)

</div>
</details>

* * *


# Q16 - 지하철(17940)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q16-1](https://leejaeseung.github.io/img/HU/Q16_1.PNG)
![Q16-2](https://leejaeseung.github.io/img/HU/Q16_2.PNG)
</div>
</details>

* * *

## 풀이

다익스트라 알고리즘으로 최단 시간을 구하는데, 우선 순위 큐를 환승 횟수로 정렬해 주어야 합니다.
환승 횟수가 같다면 짧은 시간 순으로 정렬합니다.

* * *

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {

    public static int[][] pay;
    public static boolean[] company;
    public static int[] distance;
    public static int N,M;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        pay = new int[N][N];
        company = new boolean[N];
        distance = new int[N];
        for (int i = 0; i < N ; i++) {
            distance[i] = Integer.MAX_VALUE;
        }

        for (int i = 0; i < N ; i++) {
            if(Integer.parseInt(br.readLine()) == 1)
                company[i] = true;
        }
        for (int i = 0; i < N ; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N ; j++) {
                pay[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        Dijkstra(0);
    }
    public static void Dijkstra(int start){
        PriorityQueue<tuples> pq = new PriorityQueue<>(new Comparator<tuples>() {
            @Override
            public int compare(tuples o1, tuples o2) {
                if(o1.dis > o2.dis)
                    return 1;
                else if(o1.dis == o2.dis){
                    //환승 횟수가 같을 때 짧은 시간 순서
                    return o1.pay >= o2.pay ? 1 : -1;
                }
                else
                    return -1;
            }
        });

        pq.offer(new tuples(start, 0, 0));
        distance[0] = 0;

        while(!pq.isEmpty()){
            int now = pq.peek().num;
            int now_dis = pq.peek().dis;
            int now_pay = pq.poll().pay;

            if(distance[now] < now_pay)
                continue;

            if(now == M) {
                System.out.println(now_dis + " " + now_pay);
                return;
            }

            for (int i = 0; i < N ; i++) {
                if(pay[now][i] == 0)    continue;

                if(distance[i] > now_pay + pay[now][i]){
                    distance[i] = now_pay + pay[now][i];
                    if(company[i] != company[now]){
                        //다른 회사일 경우에만 환승 횟수가 증가
                        pq.offer(new tuples(i, now_dis + 1, distance[i]));
                    }
                    else{
                        pq.offer(new tuples(i, now_dis, distance[i]));
                    }
                }
            }
        }
    }
}

class tuples{
    int num;
    int dis;
    int pay;
    public tuples(int num, int dis, int pay){
        this.num = num;
        this.dis = dis;
        this.pay = pay;
    }
}

```

![Q16-3](https://leejaeseung.github.io/img/HU/Q16_3.PNG)

</div>
</details>

* * *


# Q17 - 도미노 예측(17943)

<details>
<summary border="1" style = "font-size:1.5em;">문제</summary>
<div markdown="1">
![Q17-1](https://leejaeseung.github.io/img/HU/Q17_1.PNG)
![Q17-2](https://leejaeseung.github.io/img/HU/Q17_2.PNG)
</div>
</details>

* * *

## 풀이

풀이:
비트 연산과 세그먼트 트리(XOR) 문제입니다.  
1번 질문 : x번째 도미노에서 y번째 도미노까지 XOR한 비트의 1의 개수가 짝수 개라면 그 비트는 같습니다.  
예를 들어,
```
1
0001
2
0010
3
0011
4
0100
5
```
1~4까지 모든 XOR 계산값(0001,0010,0011)을 XOR하면 1이 모두 지워지게 되어 같은 수라는 것을 알 수 있습니다.  
따라서 x ~ y-1 까지의 모든 XOR 계산값들을 XOR 하면 1번 질문의 답이 됩니다. 단, x 와 y가 같다면 0을 출력합니다.  

2번 질문 : 1번 질문에서 구한 값의 비트(Mask) 1은 d와 다른 비트의 위치입니다.  
따라서 해당 위치의 비트를 반전시킨 값이 y가 됩니다.  
Mask 비트의 위치만 반전시키는 식은 다음과 같습니다.  
```
res = ~(xor ^ (~d))
```

반복문으로 XOR을 구하면 시간 초과가 나기 때문에 세그먼트 트리로 구간 XOR을 구합니다.  

* * * 

<details>
<summary border="1" style = "font-size:1.5em;">코드</summary>
<div markdown="1">

``` java

import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static int[] tree;
    public static int[] num;
    public static void main(String[] argc) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int Q = Integer.parseInt(st.nextToken());
        int H = (int)Math.ceil((Math.log(N - 1) / Math.log(2)));

        num = new int[N];
        tree = new int[(int)Math.pow(2, H + 1) + 1];
        st = new StringTokenizer(br.readLine());
        for (int i = 1; i <= N - 1 ; i++) {
            num[i] = Integer.parseInt(st.nextToken());
        }

        init(1, 1, N - 1);

        for (int i = 0; i < Q ; i++) {
            st = new StringTokenizer(br.readLine());
            int question = Integer.parseInt(st.nextToken());
            int x = Integer.parseInt(st.nextToken());
            int y = Integer.parseInt(st.nextToken());

            int xor = getXor(1, 1, N - 1, x, y - 1);

            if(question == 0){

                if(x != y)
                    bw.write(xor + "\n");
                else
                    bw.write(0 + "\n");
            }
            else{
                int d = Integer.parseInt(st.nextToken());
                int res = ~(xor ^ (~d));
                //Mask의 위치만 반전

                if(x != y)
                    bw.write(res + "\n");
                else
                    bw.write(d + "\n");
            }
        }
        bw.flush();
        bw.close();
    }
    public static int init(int now, int left, int right){
        if(left == right){
            return tree[now] = num[left];
        }
        return tree[now] = init(now * 2, left, (left + right) / 2) ^ init(now * 2 + 1, (left + right) / 2 + 1, right);
    }
    public static int getXor(int now, int left, int right, int start, int end){
        if(start > right || end < left) return 0;
        if(left >= start && right <= end)   return tree[now];

        return getXor(now * 2, left, (left + right) / 2, start, end) ^ getXor(now * 2 + 1, (left + right) / 2 + 1, right, start, end);
    }
}

```

![Q17-3](https://leejaeseung.github.io/img/HU/Q17_3.PNG)

</div>
</details>

* * *