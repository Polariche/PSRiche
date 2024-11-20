# DFS

### Recursive DFS

```cpp
bool visited[100];
vector<int> adjs[100];

void dfs(int a) {
    if (visited[a])
        return;
    
    visited[a] = 1;
    for (int &b : adjs[a]) {
        dfs(b);
    }
}
```



### Iterative DFS

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption><p>예시: 1 -> 3 -> 6 -> 2 -> 5 -> 4</p></figcaption></figure>

visited 테이블 + stack 을 활용한 구현

재귀호출 또한 call stack 에 파라미터를 쌓아가는 과정이므로, recursive DFS 와 동일한 순회 메커니즘을 가짐

stack 의 LIFO 구조 상, 순회 children 의 순서가 의도한 바와 반대로 나올 수 있으니 유의

```cpp
#include <vector>

// start DFS code
                
bool visited[100];
vector<int> adjs[100];
vector<int> stk;

// input neighbors into adjs

// push a starting node
stk.push_back(0);
visited[0] = 1;

// start DFS
while (!stk.empty()) {
    int a = stk.back(); stk.pop_back();
    
    // do something here
    
    for (int &b : adjs[a]) {
        if (visited[b])
            continue;
        stk.push(b);
        visited[b] = 1;
    }
}
```





### 파생 알고리즘

recursive DFS 코드에서 파생



* 백트래킹 : dfs 전 / 후에 체크 on / off

```cpp
check[b] = 1; 
dfs(b); 
check[b] = 0;
```



* 위상 정렬 : 현재 노드가 root 인 subtree 의 depth 세기

```cpp
depth = max(depth, dfs(b)+1); 
// ... 
return depth;
```



* 사이클 찾기: [tarjan.md](tarjan.md "mention")
