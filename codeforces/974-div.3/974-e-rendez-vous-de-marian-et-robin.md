---
description: '#dijkstra'
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 974 E - Rendez-vous de Marian et Robin

{% embed url="https://codeforces.com/contest/2014/problem/E" %}

### Problem Statement

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Test case #3 - graph &#x26; nodes with a horse (with orange circle)</p></figcaption></figure>



> Both Marian and Robin are capable riders, and could mount horses in no time (i.e. in $$0$$ seconds). Travel times are halved when riding. Once mounted, a horse lasts the remainder of the travel.&#x20;

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Test case #3 - 'halved' counterparts</p></figcaption></figure>



<table><thead><tr><th width="87">Node</th><th>1 -> N</th><th>1 &#x3C;- N</th></tr></thead><tbody><tr><td></td><td><img src="../../.gitbook/assets/image (11).png" alt="" data-size="original"></td><td><img src="../../.gitbook/assets/image (12).png" alt="" data-size="original"></td></tr><tr><td>1</td><td>0</td><td>30</td></tr><tr><td>2</td><td>10</td><td>25</td></tr><tr><td>3</td><td><strong>19</strong></td><td>1<strong>6</strong></td></tr><tr><td>4</td><td>27</td><td>30</td></tr></tbody></table>



두 사람이 특정 노드에서 만나는데에 걸리는 시간은 min(1->N 시간, 1 <- N 시간) 이다

Test case #3 의 경우, 3번째 노드가 min(19, 16) = 19 로 가장 이른 시각에 두 사람이 만날 수 있다



### Dijkstra

간선의 weight 가 각기 다른 그래프에서의 최단 거리를 구해야 하므로, 다익스트라 알고리즘을 활용한다



dist 테이블은 `dist[person (=여행자)][node][on_horse (=말 탑승 여부)]` 로 구성한다

* person: 두 여행자가 각 노드에 도착한 거리를 따로 기록한다
* on\_horse: Test case #4 의 경우, 1 -> 2 (말 탑승) -> 1 (말 탑승) -> 3 (말 탑승) 의 경로가 정답이다. 즉, 각 노드마다 말을 타지 않은 상태 / 말을 탄 상태로 도착한 거리를 따로 기록해야 한다

priority queue 에도 마찬가지로 (거리, 현재 노드, 여행자, 말 탑승 여부) 구조를 삽입한다

```cpp
ll dist[2][200001][2];
priority_queue<pair<pair<ll, int>, pair<bool, bool>>> pq;

while (!pq.empty()) {
    auto [res, person_and_horse] = pq.top(); pq.pop();
    auto [d, x] = res;
    auto [person, on_horse] = person_and_horse;
    // ...
}
```



> Travel times are halved when riding. Once mounted, a horse lasts the remainder of the travel.

```cpp
for (auto [nd, nx] : adj[x]) {
    if (on_horse)
        nd /= 2;
    
    ll new_dist = nd + d;
    bool newhorse = on_horse | horse[nx];
    
    // ...
}
```



> Meeting must take place on a vertex (i.e. not on an edge). Either could choose to wait on any vertex.
>
> Output the earliest time Robin and Marian can meet.

만약 상대 여행자가 현재 노드에 이미 도착한 적이 있다면 (상대방의말 탑승 여부 무관), 둘은 해당 노드에서 만남을 가질 수 있다. 다익스트라는 최단 거리를 보장하므로, 바로 해당 거리를 return 한다

<pre class="language-cpp"><code class="lang-cpp">if ((dist[!person][x][0] != -1 &#x26;&#x26; dist[!person][x][0] &#x3C;= d) ||
    (dist[!person][x][1] != -1 &#x26;&#x26; dist[!person][x][1] &#x3C;= d))
<strong>     return dist[person][x][on_horse];
</strong></code></pre>



### Solution

{% code lineNumbers="true" %}
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

#define ll long long

ll test(int T) {
	int N, M, H;
	ll dist[2][200001][2];
	priority_queue<pair<pair<ll, int>, pair<bool, bool>>> pq;
	
	cin >> N >> M >> H;
	
	bool horse[200001]={0};
	vector<pair<ll, int>> adj[200001];
	
	for (int i=0;i<=N;i++)
		dist[0][i][0] = dist[1][i][0] = dist[0][i][1] = dist[1][i][1] = -1;
	
	for (int i=0;i<H;i++) {
		int t;
		cin >> t;
		horse[t] = 1;
	}
	for (int i=0;i<M;i++) {
		int a,b;
		ll c;
		cin >> a >> b >> c;
		adj[a].push_back({c, b});
		adj[b].push_back({c, a});
	}
	
	pq.push({{0, 1}, {0, horse[1]}});
	pq.push({{0, N}, {1, horse[N]}});
	dist[0][1][horse[1]] = 0;
	dist[1][N][horse[N]] = 0;
	
	while (!pq.empty()) {
		auto [res, person_and_horse] = pq.top(); pq.pop();
		auto [d, x] = res;
		auto [person, on_horse] = person_and_horse;

		d = -d;

		if (dist[person][x][on_horse] != -1 && dist[person][x][on_horse] < d)
			continue;
			
		if ((dist[!person][x][0] != -1 && dist[!person][x][0] <= d) ||
			(dist[!person][x][1] != -1 && dist[!person][x][1] <= d))
			return dist[person][x][on_horse];
			
		for (auto [nd, nx] : adj[x]) {
			if (on_horse)
				nd /= 2;
			
			ll new_dist = nd + d;
			bool newhorse = on_horse | horse[nx];
			
			if (dist[person][nx][newhorse] != -1 && dist[person][nx][newhorse] <= new_dist)
				continue;
				
			pq.push({{-new_dist, nx}, {person, newhorse}});
			dist[person][nx][newhorse] = new_dist;
		}
	}
	
	return -1;
}


int main() {
	cin.tie(0)->sync_with_stdio(0);
	int T;
	cin >> T;
	while (T)
		cout << test(T--) << "\n";
	return 0;
}
```
{% endcode %}
