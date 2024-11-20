# Convex Hull

### Monotonic Chain

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#define ll long long 

struct Pos {
    ll x;
    ll y;
    bool operator<(const Pos &other) const {
        return other.x==x?y<other.y:x<other.x;
    }
};

bool ccw(Pos a, Pos b, Pos c) {
    // det (b-a, c-b) < 0 => CCW
    return (b.x - a.x)*(c.y - b.y) - (b.y - a.y)*(c.x - c.y) < 0;
}

vector<Pos> convex_hull (vector<Pos> vec) {
    vector<Pos> cvh;
    int tmp;
    int N = vec.size();
    
    for (int i=0;i<N;i++) {
        while (cvh.size() >= 2 && !ccw(cvh[cvh.size()-2], cvh[cvh.size()-1], vec[i]))
            cvh.pop_back();
        cvh.push_back(vec[i]);   
    }
    
    tmp = cvh.size();
    for (int i=N-1;i>=0;i--) {
        while (cvh.size()-tmp >= 2 && !ccw(cvh[cvh.size()-2], cvh[cvh.size()-1], vec[i]))
            cvh.pop_back();
        cvh.push_back(vec[i]);   
    }
    
    return cvh;
}
```



### References

{% embed url="https://bamgoesn.github.io/posts/algorithms/2023-09-11-monotone-chain-cvh/" %}
