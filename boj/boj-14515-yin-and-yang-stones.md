---
description: '#adhoc'
---

# BOJ 14515 - Yin and Yang Stones

{% embed url="https://www.acmicpc.net/problem/14515" %}



### Problem Statement

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption><p>Image credit : <a href="https://www.acmicpc.net/user/wizardrabbit">wizardrabbit</a></p></figcaption></figure>

검은 돌과 하얀 돌이 만나면 충돌하여 사라진다



{% tabs %}
{% tab title="Sol1" %}
Double linked list 를 구현하여 푼 해답

{% code lineNumbers="true" %}
```cpp
#include <iostream>
using namespace std;

struct bead {
    bool col;
    bead* prev;
    bead* next;
};

int main() {
    cin.tie(0)->sync_with_stdio(0);
    string s;
    int n;
    cin >> s;
    bead* start;
    bead* cur;
    int p = 0;
    
    n = s.size();
    
    if (n%2) {
        cout << 0;
        return 0;
    }
    
    for (int i=0;i<n;i++) {
        bead* b = new bead();
        b->col = (s[i]=='W');
        
        if (i==0)
            start = b;
        else {
        	if (i==n-1) {
        		start->prev = b;
        		b->next = start;
        	}
        		
        	cur->next = b;
        	b->prev = cur;
        }
        	
        cur = b;
    }
    cur = start;
    
    for (int i=0;i<n;i++) {
    	
    	if (cur->col != cur->next->col) {
    		cur->next = cur->next->next;
    		cur->prev->next = cur->next;
    		cur->next->prev = cur->prev;
    		p++;
    		cur = cur->prev;
    		if (p == n/2) {
    			cout << 1;
    			return 0;
    		}
    	} else {
            cur = cur->next;
        }
    	
    }
    cout << 0;
}
```
{% endcode %}
{% endtab %}

{% tab title="Sol2" %}
B의 수 == W의 수 인지 확인

{% code lineNumbers="true" %}
```cpp
#include <iostream>
using namespace std;

int main() {
    cin.tie(0)->sync_with_stdio(0);
    string s;
    int n;
    cin >> s;
    
    int b = 0;
    int w = 0;
    
    for (char c : s) {
    	if (c=='W')
    		w++;
    	else
    		b++;
    }
    
    cout << (w==b);
}
```
{% endcode %}
{% endtab %}
{% endtabs %}



### Proof

순환 문자열이고, 가능한 문자가 B 와 W 밖에 없으므로 BW가 한 군데에는 존재한다

해당 BW 를 삭제하여 생성된문자열에도 위 서술이 동일하게 적용된다

따라서, B와 W의 수만 같다면 연쇄 반응에 의해 모든 돌이 사라지게 된다
