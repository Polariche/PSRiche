---
description: Weighted graph 와 시작점이 주어졌을 때, 시작점으로부터 나머지 노드로 가는 최단거리를 구하는 알고리즘이다
---

# Dijkstra

Dijkstra 는 BFS 코드에서 두 가지 변경사항을 적용하여 구현가능하다

* queue -> priority\_queue
  * c++ 기준, priority\_queue 는 max heap 이므로 {-dist, index} 형태로 입력
* (dist\[ni] >= 1) -> (dist\[ni] >= edge(i, ni) + dist\[i]) 체크

{% tabs %}
{% tab title="Dijkstra" %}
<pre class="language-cpp"><code class="lang-cpp"><strong>int start;
</strong>int dist[100];    // init with INF
vector&#x3C;pair&#x3C;int, int>> adjs[100];
priority_queue&#x3C;pair&#x3C;int, int>> pq;

pq.push({-0, start});
dist[start] = 0;

while (!pq.empty()) {
    auto [d, i] = pq.top(); pq.pop();
    d=-d;
    
    if (d > dist[i])
        continue;
    
    for (auto [nd, ni] : adjs[i]) {
        int new_dist = nd+dist[i];
        if (new_dist >= dist[ni])
            continue;
        pq.push({-new_dist, ni});
        dist[ni] = new_dist;
    }
}
</code></pre>
{% endtab %}

{% tab title="BFS" %}
```cpp
int start;
int dist[100] = {0};
vector<int> adjs[100];
queue<int> q;

q.push(start);
dist[start] = 1;

while (!q.empty()) {
    int i = q.front(); q.pop();
    
    
    
    
    
    for (int &ni : adjs[i]) {   
             
        if (dist[ni] >= 1)
            continue;
        q.push(ni);
        dist[ni] = dist[i] + 1;   
    }
}
```
{% endtab %}
{% endtabs %}



BFS 는 edge weight 이 모두 동일한 특수 케이스로 생각할 수 있다

\-> 입력 받는 노드는 항상 도달 거리 순으로 들어오는 것이 보장되어 있으므로, priority queue 가 아니라 queue 로 처리가 가능하다
