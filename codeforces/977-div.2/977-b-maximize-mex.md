# 977 B - Maximize Mex

{% embed url="https://codeforces.com/contest/2021/problem/B" %}

### Congruence

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Test case #2</p></figcaption></figure>

```cpp
bool comp(ll x, ll y) {
	if (x%op == y%op)
		return x<y;
	return x % op < y % op;
}
```

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Test case #2 -> binned by remainder &#x26; sorted</p></figcaption></figure>

`mex % op`  는 0 -> 1 -> 2 -> ... -> op-1 -> 0 -> 1 -> ... 순으로 변하니, 정렬한 배열의 구역을 순회하며, 배열의 값이 현재 mex 보다 작거나 같은 경우 해당 수를 소모해주면 된다.

### Solution

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <map>
using namespace std;

#define ll long long

ll op;

bool comp(ll x, ll y) {
	if (x%op == y%op)
		return x<y;
	return x % op < y % op;
}

int test() {
	ll N;
	ll arr[200001];
	map<ll, ll> idx;
	map<ll, ll> idx_end;
	
	ll mex;
	
	cin >> N >> op;
	for (int i=0;i<N;i++)
		cin >> arr[i];
	
	sort(arr, arr+N, comp);
	
	for (ll i=0;i<N;i++) {
		if (idx.find(arr[i]%op) == idx.end())
			idx[arr[i]%op] = i;
		idx_end[arr[i]%op] = i;
	}
	
	mex = 0;
	while (mex < N) {
		ll m = mex % op;
		if (arr[idx[m]] <= mex && idx[m] <= idx_end[m] && (arr[idx[m]]%op == m)) {
			idx[m]++;
			mex++;
		} else {
			break;
		}
	}
	
	return mex;
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



