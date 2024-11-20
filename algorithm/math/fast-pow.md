# Fast pow

Divide-and-conquer

```cpp
#define ll long long
#define M 1000000007

ll pow(ll base, ll ex) {
	if (ex==0)
		return 1;
	if (ex%2)
		return ((pow(base, ex-1)%M)*base)%M;
		
	ll res = (pow(base, ex/2))%M;
	return (res*res)%M;
}
```
