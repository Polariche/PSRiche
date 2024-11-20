# Union - Find

그래프에서 Connected component 를 찾는 알고리즘

각 edge 마다 `uni(x, y)` 를 실행 \
\-> g\[x] 에 connected component 중 가장 작은 값의 원소가 채워짐

```cpp
// init with g[i] = i
int g[30001];		

int find(int x) {
	if (x==g[x])
		return g[x];
	return g[x]=find(g[x]);
}

void uni(int x, int y) {
	x=find(x);
	y=find(y);
	g[x]=g[y]=min(x,y);
}
```
