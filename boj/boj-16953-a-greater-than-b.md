# BOJ 16953 - A -> B



{% embed url="https://www.acmicpc.net/problem/16953" %}

A -> B 로 변환하는 데에 두 가지 방법을 사용 가능

* 2로 곱하기
* 1을 수의 오른쪽에 추가



거꾸로생각해서, B -> A 로 가는 방법

* 2로 나누기 (2의 배수일 때)
* 10으로 나누기 (10의 나머지가 1일 때)

{% tabs %}
{% tab title="Sol1" %}
Greedy&#x20;

{% code lineNumbers="true" %}
```python
import sys
input = sys.stdin.readline

a,b = map(int,input().split())
ans = 0
while b>a:
    if b%10 == 1:
        b = b//10
    elif b%2 == 0:
        b = b//2
    else:
        print(-1)
        exit()
    ans += 1
print(ans+1 if a==b else -1)
```
{% endcode %}
{% endtab %}

{% tab title="Sol2" %}
BFS

{% code lineNumbers="true" %}
```python
import sys
input = sys.stdin.readline
from collections import deque

a,b = map(int,input().split())
q = deque([a])
visited = {a: 1}

def add(x, cnt):
	if x <= b and x not in visited.keys():
		visited[x] = cnt+1
		q.append(x)
		
while q:
	c = q.popleft()
	if c == b:
		print(visited[c])
		exit()
		
	add(2*c, visited[c])
	add(10*c+1, visited[c])
	
print(-1)
```
{% endcode %}
{% endtab %}
{% endtabs %}
