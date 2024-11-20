# Binary Search

### Iterative

```cpp
int arr[N];
int target;

// input ...

int s=0, e=N-1;

sort(arr, arr+N);

while (s<=e) {
    int m = (s+e)/2;
    
    if (arr[m] == target)
        break;
    else if (arr[m] < target)
        s = m+1;
    else if (arr[m] > target)
        e = m-1;
}
```



### Recursive

