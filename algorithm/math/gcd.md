# GCD

```cpp
int gcd(int x, int y) { // assume x>y
    if (!y)
        return x;
    return gcd(y,x%y);
}
```
