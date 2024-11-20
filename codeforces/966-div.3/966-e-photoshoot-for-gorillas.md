# 966 E - Photoshoot for Gorillas

{% embed url="https://codeforces.com/contest/2000/problem/E" %}

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### 요약

N x M 칸에 size 가 K 인 커널로 컨볼루션

각 칸에 $$a_1, a_2, ..., a_w$$ 를 자유롭게 배치할 수 있을 때, 컨볼루션의 최대값 구하기



컨볼루션에 의해 각 칸의 값에 곱해지는 coefficient 를 먼저 구하기

\-> coefficient 와 $$a_1, a_2, ..., a_w$$ 를 내림차순으로 정렬하여 곱한 후 더하면 정답



### Coefficient 구하기

1D 컨볼루션 기준, 각 항의 최대 coeff 는 K

변두리와 가까운 항 (0-index 기준: i=0, 1, 2, ..., K-2)  은 각각 coeff 가 1, 2, 3, ..., K-1 로 증가하는 형태

반대편 (i=N-1, N-2, N-3, ..., N-K+1) 도 마찬가지로 coeff 가 1, 2, 3, ..., K-1 로 증가

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

마지막 예제 (N=5, K=5) 와 같은 경우, index 가 K 에 도달하기 전에 반대쪽 변두리에 도달

\-> coeff 의 최대 값은 N-K+1



최종 식은

```cpp
 min(min(x+1, min(N-K+1, K)), N-x)
```

2D 컨볼루션이니, column 방향에 대해서도 동일 식 계산 후 (row 식)\*(col 식) 으로 곱하기



### Solution

{% code lineNumbers="true" %}
```cpp
#include <iostream>
#include <vector>
#include <set>
#include <algorithm>
using namespace std;
#define ll long long


void test() {
	int N,M,K;
	int W;
	
	ll go[200001]={0};
	ll coefs[200001];
	ll ans=0;
	
	cin >> N >> M >> K;
	cin >> W;
	for (int i=0;i<W;i++) {
		cin >> go[i];
	}
	for (int i=0;i<N*M;i++) {
		int x = i/M, y=i%M;
		coefs[i] = min(min(x+1, min(N-K+1, K)), N-x)*min(min(y+1, min(M-K+1, K)), M-y);
		//cout << x << " " << y << " " << coefs[i] << "\n";
	}
	
	sort(coefs, coefs+N*M, greater<int>());
	sort(go, go+N*M, greater<int>());
	
	for (int i=0;i<N*M;i++)
		ans += coefs[i]*go[i];
	cout << ans << "\n";
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
{% endcode %}





