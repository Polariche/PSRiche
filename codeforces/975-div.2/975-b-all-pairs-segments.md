---
description: '#sweeping #imos'
---

# 975 B - All Pairs Segments

{% embed url="https://codeforces.com/contest/2019/problem/B" %}

### Problem Statement

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption><p>Test case #2</p></figcaption></figure>

### Sweeping&#x20;

$$0 \leq i < j < n$$  조건에 의해, 다음 규칙이 존재한다 (i, j 는 0-인덱스 사용)

* $$x_i$$ 부터 시작하는 segment 는 $$N-i-1$$ 개 존재한다
* $$x_j$$ 에서 종료되는 segment 는 $$j$$ 개 존재한다

이를 활용하여, 각 $$x_i$$ 에서 몇 개의 segment 가 생성되고 소멸되는 지를 기록할 수 있다

```cpp
for (int i=0;i<N;i++) {
    diffs.push_back({pts[i], {N-i-1, -i}});
}
```



배열을 스위핑하며, 현재 $$x_i$$ 가 포함되어 있는 segment 의 수를 업데이트한다

segment 는 inclusive 하므로, 생성 segment 수 더하기 -> 기록 ->  소멸  segment 수 빼기 순서를 따라야 한다

<pre class="language-cpp"><code class="lang-cpp">ll lastval = diffs[0].first;
<strong>ll lastcnt = 0;	
</strong>for (auto [value, d] : diffs) {
	// ...
	lastcnt += d.first;
	
	if (ans.find(lastcnt) == ans.end())
		ans[lastcnt] = 0;
	ans[lastcnt]++;
	
	lastcnt += d.second;
}
</code></pre>



만약  $$x_i$$ 와  $$x_{i+1}$$ 사이에 다른 수가 존재했다면, 그 수만큼 현재 segment 수에 더한다

```cpp
if (value - lastval > 1) {
	if (ans.find(lastcnt) == ans.end())
		ans[lastcnt] = 0;
	ans[lastcnt] += value-lastval-1;
	//cout << value - lastval - 1 << " " << lastcnt << "\n";
}
```



### Solution

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <map>
using namespace std;

#define ll long long


void test () {
	ll N, Q;
	cin >> N >> Q;
	
	vector<ll> pts(N);
	vector<ll> questions(Q);
	vector<pair<ll, pair<int, int>>> diffs;
	ll lastval = -1;
	ll lastcnt = 0;
	map<ll, ll> ans;
	
	for (int i=0;i<N;i++)
		cin >> pts[i];
	for (int i=0;i<Q;i++)
		cin >> questions[i];
	
	sort(pts.begin(), pts.end());
	
	for (int i=0;i<N;i++) {
		diffs.push_back({pts[i], {N-i-1, -i}});
	}
	
	sort(diffs.begin(), diffs.end());
	lastval = diffs[0].first;
	
	for (auto [value, d] : diffs) {
		
		if (value - lastval > 1) {
			if (ans.find(lastcnt) == ans.end())
				ans[lastcnt] = 0;
			ans[lastcnt] += value-lastval-1;
			//cout << value - lastval - 1 << " " << lastcnt << "\n";
		}
		
		lastcnt += d.first;
		
		if (ans.find(lastcnt) == ans.end())
			ans[lastcnt] = 0;
		ans[lastcnt]++;
		
		//cout << value << " " << lastcnt << " " << d.first << " " << d.second << "\n";
		lastcnt += d.second;
		lastval = value;
	}
	
	for (ll q : questions) {
		if (ans.find(q) == ans.end())
			cout << 0 << " ";
		else
			cout << ans[q] << " ";
	}
	
	cout << "\n";
}

int main() {
	cin.tie(0)->sync_with_stdio(0);
	int T;
	cin >> T;
	while (T--)
		test();
	
	return 0;
}
```





