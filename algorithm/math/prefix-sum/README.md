# Prefix Sum

$$
S_i = \sum_{j=1}^{i}{a_j}
$$

creation: O(N)

```cpp
int sum[101]={0};
for (int i=1;i<=100;i++) {    // use 1-idx for simpler code
    int x;
    cin >> x;
    sum[i] = sum[i-1]+x;
}
```



$$
\sum_{i=s}^{e}{a_i} = \sum_{i=1}^{e}{a_i} - \sum_{i=1}^{s-1}{a_i}
$$

querying: O(1)

```cpp
// A(s) + A(s+1) + ... + A(e-1) + A(e)
cout << sum[e] - sum[s-1];
```
