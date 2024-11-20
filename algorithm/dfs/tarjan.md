# Tarjan

Directed graph 에서 strongly connected component  (SCC) 를 찾는 데에 쓰는 알고리즘

```cpp
int V,E;
vector<int> adj[10001];
vector<int> stk;

int v[10001] = {0};
int cycle[10001] = {0};

map<int, vector<int>> ans;

int dfs(int x) {
    if (v[x] && !cycle[x])
        return v[x];
    if (cycle[x])
        return 10003;

    int res = v[x] = ++v[0];
    stk.push_back(x);

    for (int y : adj[x])
        // if we come across a node with smaller v, a cycle is created
        res = min(res, dfs(y));
    
    // take out the cycle components from the stack
    if (v[x] == res) {
        vector<int> scc;
        while (!stk.empty()) {
            scc.push_back(stk.back());
            stk.pop_back();
            if (scc.back() == x) break;
        }
        for (int y : scc)
            cycle[y] = x;
    }
    
    return res;
}
```

각 노드마다 고유번호 (v) 를 방문 순서대로 매기고, 추후 dfs 에서 이전에 이미 방문한 노드 (= v 가 더 작은 노드) 를 만났을 시 사이클을 형성한다.&#x20;

해당 노드를 사이클의 root 노드로 취급하며, 만약 여러 사이클을 만났을 시엔 v 가 최소인 노드를 선택하여 사이클의 크기를 최대화한다.

현재 노드가 root 노드라면, 스택의 끝 노드 \~ cycle root 노드 까지를 사이클로 묶는다&#x20;

1 -> 2 -> 3 -> 4 -> 5 -> 3 순으로 방문 시, 스택 내 \[3, 4, 5] 까지가 한 사이클로 묶이게 된다
