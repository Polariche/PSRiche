---
description: '#binary_search #greedy #longlong #nlogn'
---

# 974 C - Robin Hood in Town

{% embed url="https://codeforces.com/contest/2014/problem/C" %}

### Problem Statement

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption><p>half avg 보다 작거나 같으면 unhappy, 크면 happy</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption><p>half avg 를 올리면, 재산이 적은 happy 부터 unhappy 로 순차적으로 변함</p></figcaption></figure>

### Edge Cases

> &#x20;find an $$a_j=max(a_1,a_2,…,a_n)$$, change $$a_j$$ to $$a_j+x$$
>
> A person is unhappy if their wealth is strictly less than half of the average wealth$$∗$$.

위 문제 조건에 의해,  가장 많은 재산을 가진 사람은 항상 happy 하다



> If **strictly more than half of the total population** $$N$$ are unhappy, Robin Hood will appear by popular demand.
>
> ... or output $$−1$$−1 if it is impossible.

* $$N = 1$$ : happy 1명으로 로빈 후드가 나타날 수 없다&#x20;
* $$N=2$$ : $$x$$ 를 최대한 올려도, unhappy 1명, happy 1명으로 로빈 후드가 나타날 수 없음

그러므로, 위 두 경우에 -1 를 리턴



&#x20;$$N \geq 2$$ : $$x$$ 가 무한대로 커지면 unhappy N-1 명, happy 1명이 남기에 로빈 후드가 나타날 수 있다



### Using Integers

* 정수형 (int, long long) 범위 vs 실수형 범위
* 실수는 정확도 부족 -> 수식 (avg = sum/N 관련) 을 정수로 변환
* $$Na_i \leq 2\cdot10^6 \cdot 10^5$$



### Binary Search

현재 입력 기준으로 이미 "unhappy" 인 사람의 수 필요

\-> 정렬 후 binary search 로 "half of average" 보다 작은 최대 값을 구함

```cpp
sort(a.begin(), a.end());
// ...
while (s<=e) {
	int m = (s+e)/2;
	if (a[m]*2*N < sum) {
		angry_idx = m;
		s = m+1;
	} else
		e = m-1;
}
```



### Greedy

happy 한 사람을 unhappy 로 변환하기 위한 $$x$$ 의 값은 $$2N\cdot a_i - sum([a_i]) + 1$$

이는 $$a_i$$ 가 작을 수록 작은 값임

> Determine the minimum value of $$x$$x for Robin Hood to appear

\-> 문제 조건대로 최소 $$x$$ 를 구하기 위해서 그리디 활용, 배열 순 (= 오름차순) 대로 값 구하기

```cpp
ll extra = 0;
int i = angry_idx+1;
while (i <= N/2) {
	extra = a[i]* 2*N - sum+1;
	i++;
}

return extra;
```



### Solution

{% code lineNumbers="true" %}
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>
using namespace std;
#define ll long long
ll test() {
	int N;
	cin >> N;
	vector<ll> a(N);
	ll sum =0;
	
	for (int i=0;i<N;i++)
		cin >> a[i];
		
	if (N<=2)
		return -1;
	
	sort(a.begin(), a.end());
	
	for (int b : a)
		sum += b;
		
	
	a.pop_back();
	
	int s=0, e=a.size()-1;
	int angry_idx = -1;
	int angry_total;
	
	while (s<=e) {
		int m = (s+e)/2;
		if (a[m]*2*N < sum) {
			angry_idx = m;
			s = m+1;
		} else
			e = m-1;
	}
	
	ll extra = 0;
	int i = angry_idx+1;
	while (i <= N/2) {
		extra = a[i]* 2*N - sum+1;
		i++;
	}
	
	return extra;
}

int main() {
	cin.tie(0)->sync_with_stdio(0);
	int T;
	cin >> T;
	while (T--)
		cout << test() << "\n";
	return 0;
}
```
{% endcode %}
