# Cramer's Rule

2차 연립방정식의 해를 구하는 공식

```cpp
ll cross2 (ll x1, ll y1, ll x2, ll y2) {
	return x1*y2-y1*x2;
}

void cross3 (ll x1, ll y1, ll z1,
             ll x2, ll y2, ll z2,
             ll *x, ll *y, ll *z) {
				
	*x = cross2(y1, z1, z1, z2); 
	*y = cross2(z1, x1, z2, x2); 
	*z = cross2(x1, y1, x2, y2); 
}

int main () {
      // ...
      cross3(x1, y1, -z1,   x2, y2, -z2,   &x3, &y3, &z3);
      if (z3 != 0) {
            // a solution exists
            x3 /= z3;
            y3 /= z3;
      } else if (x3 == y3){
             // infinite solutions
      } else {
            // no solution
      }
}
```





### Dual w/ 3D Geometry

3차원 좌표계에서,

* n1 (x1, y1, z1) 를 지나고 O  - n1 에 수직한 평면
* n2 (x2, y2, z2)를 지나고 O  - n2 에 수직한 평면

두 평면을 교차시키면 직선  O - u 가 나온다.&#x20;

벡터 u는 n1 과 n2 를 외적하여 구할 수 있다

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p><a href="https://www.youtube.com/watch?v=dJIIa6-WW0o">https://www.youtube.com/watch?v=dJIIa6-WW0o</a></p></figcaption></figure>

n1 와 n2 가 평행하지 않다는  가정 하에, 직선  O - u 는 다음 연립방정식의 해공간을 의미한다

* x1 x + y1 y + z1 z = 0
* x2 x + y2 y + z2 z = 0



<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

z = 1 로 고정시켜서, 다음 2차 연립방정식의 해를 획득할 수 있다

* x1 x + y1 y = -z1
* x2 x + y2 y = -z2

직선 O - u 위 점 중, z=1 인 점이 위 2차 연립방정식의 해가 된다

u(x3, y3, z3) -> u'(x3/z3, y3/z3, 1)





### Related Problems

* [크래머의 공식](https://www.acmicpc.net/problem/7561)
* [선분 교차 2](https://www.acmicpc.net/problem/17387)

