# BFS

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption><p>예시: 1 -> 2 -> 3 -> 4 -> 5 -> 6</p></figcaption></figure>

**인접한 노드부터 탐색**

visited 테이블 + queue 를 활용하여 구현



### 그래프 순회 BFS

```cpp
#include <queue>

// start BFS code

bool visited[100];
vector<int> adjs[100];
queue<int> q;

// input adjs, ...

// push a starting node
q.push(0);
visited[0] = 1;

// start BFS
while (!q.empty()) {
    int a = q.front(); q.pop();
    
    // do something here
    
    for (int &b : adjs[a]) {
        if (visited[b])
            continue;
        q.push(b);
        visited[b] = 1;
    }
}
```



### 2차원 grid BFS

2차원 grid BFS는 따로 인접 정보를 받아오지 않는다.&#x20;

좌표 (x, y) 는 4방향 (위, 아래, 왼쪽, 아래) 에 위치한 좌표 (x-1, y), (x+1, y), (x, y-1), (x, y+1) 와 인접하고 있단 사실을 활용한다

```cpp
// ^, V, <, >
int dx[4] = {-1,1,0,0};
int dy[4] = {0,0,-1,1};

// N = rows, M = cols
int N,M;                    
bool visited[100][100];
queue<int> q;

// input N and M

q.push(0);
visited[0][0] = 1;

while (!q.empty()) {
    int a = q.front(); q.pop();
    int x = a/M;
    int y = a%M;
    
    // do something here
    
    for (int i=0;i<4;i++) {
        int nx = x+dx[i];
        int ny = y+dy[i];
        if (nx < 0 || ny < 0 || nx >= N || ny >= M)
            continue;
        if (visited[nx][ny])
            continue;
        // insert additional condition according to the problem
        
        q.push(nx*M + ny);
        visited[nx][ny] = 1;
    }
}
```





### 최단거리로 목적지 도달하기

BFS 는 가장 인접한 노드부터 탐색&#x20;

\-> 노드 X 로부터 노드 Y 까지 항상 최단거리로 도달하는 것이 보장되어 있다



visited 행렬에 도달 거리를 기록하면 최단거리(혹은 시간)을 구할 수 있다

```cpp
visited[b] = visited[a] + 1;
```





### 파생 최단거리 알고리즘

모든 간선의 가중치가 동일할 때에만 BFS 로 최단거리를 구할 수 있다

만약 가중치가 다를 때에는 (=weighted graph) 다른 알고리즘을 활용해야 한다

* [dijkstra.md](dijkstra.md "mention") (weight > 0)\
  : queue 대신 priority\_queue\<pair<...>> 사용,\
  &#x20; visited 대신 dist 사용; dist\[v] > dist\[u]+edge(u, v) 일시 enqueue
* Bellman-Ford Algorithm
