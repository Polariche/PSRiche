---
description: '#backtracking'
---

# BOJ 9663 - N-Queen

{% embed url="https://www.acmicpc.net/problem/9663" %}

{% tabs %}
{% tab title="Sol1 (TLE)" %}
O((N^2)!) 탐색 \* O(N) 배치

총 시간복잡도는 O(N \* (N^2)!) -> 시간 초과

```python
n = int(input())
a = [[0 for _ in range(n)] for _ in range(n)]

def place(x,y,t):
    for i in range(0,n):
        a[x][i] += t
        a[i][y] += t
    if x<=y:
        for i in range(n-y+x):
            a[i][y-x+i] += t
            a[y-x+i][-1-i] += t
    else:
        for i in range(n-x+y):
            a[x-y+i][i] += t
            a[i][x-y-i] += t

cnt = 0
def hey(i):
    global cnt
    if cnt == n:
        return 1
    res = 0
    for j in range(i,n*n):
        x,y=j//n,j%n
        if a[x][y] > 0:
            continue
        place(x,y,1); cnt+=1
        res += hey(j+1)
        place(x,y,-1); cnt-=1
    return res

print(hey(0))
```
{% endtab %}

{% tab title="Sol2" %}
O(N^N) 탐색 \* O(N) 배치

총 시간복잡도는 O(N \* (N^N))&#x20;

```python
import sys
n = int(sys.stdin.readline())
row = [-1 for _ in range(n)]
def atk(r):
    for i in range(r):
        if row[i] == row[r] or abs(i-r) == abs(row[i]-row[r]):
            return False
    return True
def queen(r):
    if r == n:
        return 1
    ans = 0
    for i in range(n):
       row[r] = i
       if atk(r):
            ans += queen(r+1)
    return ans
print(queen(0))
```

구글링 풀이 참조 -> 모든 row 에 queen 이 하나씩 존재해야 함

queen(r) 에서 r 번째 row 의 queen 위치를 지정 -> atk(r) 에서 현재 놓으려는 위치 (row\[r]) 가 올바른지 확인

올바르면 해당 위치에 queen 을 놓고 다음 row 로 넘어감
{% endtab %}

{% tab title="Sol3" %}
O(N^N) 탐색 \* O(1) 배치

```cpp
#include <iostream>
using namespace std;
#define ll long long

int N;
bool d[16]={0};
bool d1[32]={0};
bool d2[32]={0};

ll check(int i, int cnt) {
    int x=i/N;
    int y=i%N;
    ll res=0;
    
    if (cnt<=x-1)
    	return 0;
    if (i>=N*N || cnt==N)
        return cnt==N;
    
    if (d[y] || d1[x+y] || d2[x-y+N-1])
        return check(i+1, cnt);
    
    d[y] = d1[x+y] = d2[x-y+N-1] = 1;
    res += check((x+1)*N, cnt+1);
    d[y] = d1[x+y] = d2[x-y+N-1] = 0; 
    
    res += check(i+1, cnt);
    
    return res;
}

int main() {
    cin.tie(0)->sync_with_stdio(0);
    cin >> N;
    cout << check(0, 0);
}
```

배열 d, d1, d2 에 각각 다음의 정보를 기록

* d: 각 열에 queen 이 존재하는지
* d1 : 각각의 오른쪽 위 방향 대각선에 queen 이 존재하는지
* d2: 각각의 왼쪽 위 방향 대각선에 queen 이 존재하는지

정사각행렬의 대각선의 인덱스는 그림에 나타난 수식을 통해 획득 가능

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

현 위치에 대해 d, d1, d2 를 체크함으로써 queen 의 배치 가능 여부를 O(1) 에 확인 가능

배치 이후 직후엔 현 위치에 대한 d,d1,d2 = 1 을 지정하여 위치 점유 \
\-> 다음 경우의 수 확인 이후엔 다시 queen 을 꺼내어 d,d1,d2 = 0&#x20;

```cpp
    d[y] = d1[x+y] = d2[x-y+N-1] = 1;
    res += check((x+1)*N, cnt+1);
    d[y] = d1[x+y] = d2[x-y+N-1] = 0; 
    
    res += check(i+1, cnt);
```

queen 을 배치했다면 다음 row 로 넘어감 ( i = x\*N + y -> (x+1)\*N )

만약 배치하지 않았다면 다음 col 로 넘어감 ( i -> i+1 )

```cpp
    if (cnt<=x-1)
    	return 0;
    if (i>=N*N || cnt==N)
        return cnt==N;
```

만약 다음 row 로 넘어갔는데 queen 의 수가 row 보다 적다면 실패 -> 탐색 중지

index 의 끝에 도달했거나 queen 의 수가 N개에 도달 -> 성공 시 (queen 의 수 == N) 결과에 +1
{% endtab %}
{% endtabs %}





