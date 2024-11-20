---
description: '#string #cumulative'
---

# BOJ 31772 - Logical Moos

{% embed url="https://www.acmicpc.net/problem/31772" %}

{% tabs %}
{% tab title="Sol1 (fail)" %}
각 쿼리마다 새로 evaluation 해보는 방식 -> O(QN)

Q, N <= 10^5 이므로 당연히 timeout

{% code lineNumbers="true" %}
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main () {
    cin.tie(0)->sync_with_stdio(0);
    int N, Q;
    char ex[200002];
    cin >> N >> Q;
    for (int i=1;i<=N;i++) {
        string s;
        cin >> s;
        switch(s[0]) {
            case 't': ex[i]=3; break;
            case 'f': ex[i]=0; break;
            case 'a': ex[i]='&'; break;
            case 'o': ex[i]='|'; break;
        }
    }
    while (Q--) {
        int a,b, res, target, i;
        char temp;
        string s;
        cin >> a >> b >> s;
        
        
	    vector<char> stk_value;
	    vector<char> stk_op;
        
        if (s[0] == 't')
        	target = 1;
        else
        	target = 0;
        	
        temp = ex[a];
        ex[a] = 2;
        	
        for (i=1;i<=N;i++) {
        	//cout << i << " " << ex[i] << "\n";
            switch(ex[i]) {
                case '&': stk_op.push_back('&'); break;
                case '|': 
                    while (!stk_op.empty() && stk_op.back() == '&') {
                        char v1, v2;
                        stk_op.pop_back();
                        v1 = stk_value.back(); stk_value.pop_back();
                        v2 = stk_value.back(); stk_value.pop_back();
                        stk_value.push_back(v1 & v2);
                    }
                    stk_op.push_back('|');
                    break;
                default: stk_value.push_back(ex[i]);
            }
            
            if (a==i)
            	i=b;
        }
        while (!stk_op.empty()) {
            char v1, v2, op;
            v1 = stk_value.back(); stk_value.pop_back();
            v2 = stk_value.back(); stk_value.pop_back();
            op = stk_op.back(); stk_op.pop_back();
            switch(op) {
                case '&': stk_value.push_back(v1 & v2); break;
                case '|': stk_value.push_back(v1 | v2); break;
            }
        }
        res = stk_value.back();
        cout << ((res/2 == target || res%2 == target) ? 'Y' : 'N');
        
        ex[a] = temp;
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Sol2 (fail)" %}
inorder 표기로부터 expression tree 를 생성하고, 각 쿼리마다 evaluation 해보는 방식

각 노드에 index 를 기록해 두어, q1 <= index <= q2 인 노드는 스킵

실패 이유: O(N), O(Q log N) 구현이나, worst case 에 O(QN) 이 될 수 있음

\-> 입력이... and ... and ... and ... and ... and ... and 일 시 tree depth 가 O(log N) 이 아닌 O(N) 이 됨

{% code lineNumbers="true" %}
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Node {
	public:
		int idx;
		char val;
		Node* left;
		Node* right;
		
		Node (int _idx, char _val);
		int eval(int a, int b);
};

Node::Node(int _idx, char _val) {
	idx = _idx;
	val = _val;
}

int Node::eval(int a, int b) {
	//cout << idx << " " << val << "\n";
	
	if (a<=idx && idx<=b) {
		if (val == '&' || val == '|') {
			int nochange = val=='&' ? 3 : 0;	// T & x = x, F | x = x
			int l_idx = left->idx;
			int r_idx = right->idx;
			int l_res = (a <= l_idx && l_idx <= b)? nochange : left->eval(a,b);
			int r_res = (a <= r_idx && r_idx <= b)? nochange : right->eval(a,b);
			
			if (val=='&')
				return l_res & r_res;
			else if (val=='|')
				return l_res | r_res;
		} else
			return 2;
	} else {
		switch(val) {
			case '&': return left->eval(a, b) & right->eval(a,b);
			case '|': return left->eval(a, b) | right->eval(a,b);
			case 'T': return 3;
			case 'F': return 0;
			default: return val;
		}
	}
	
}

int main () {
    cin.tie(0)->sync_with_stdio(0);
    int N, Q;
    char ex[200002];
    
    vector<Node*> nodes_val;
    vector<Node*> nodes_op;
    
    cin >> N >> Q;
    for (int i=1;i<=N;i++) {
        string s;
        Node *node;
        cin >> s;
        
        if (s[0] == 't')
        	node = new Node(i, 'T');
        else if (s[0] == 'f')
        	node = new Node(i, 'F');
        else if (s[0] == 'a')
        	node = new Node(i, '&');
        else if (s[0] == 'o')
        	node = new Node(i, '|');
        	
        if (node->val == '|') {
        	while (!nodes_op.empty() && nodes_op.back()->val == '&') {
        		Node* n1 = nodes_val.back(); nodes_val.pop_back();
        		Node* n2 = nodes_val.back(); nodes_val.pop_back();
        		Node* n_op = nodes_op.back(); nodes_op.pop_back();
        		
        		n_op->left = n2;
        		n_op->right = n1;
        		nodes_val.push_back(n_op);
        	}
        	nodes_op.push_back(node);
        	
        } else if (node->val == '&') {
        	nodes_op.push_back(node);
        	
    	} else {
        	nodes_val.push_back(node);
        }
    }
    
    while (!nodes_op.empty()) {
		Node* n1 = nodes_val.back(); nodes_val.pop_back();
		Node* n2 = nodes_val.back(); nodes_val.pop_back();
		Node* n_op = nodes_op.back(); nodes_op.pop_back();
		
		n_op->left = n2;
		n_op->right = n1;
		nodes_val.push_back(n_op);
	}
    
    while (Q--) {
    	int q1, q2, res, qc;
    	string query;
    	cin >> q1 >> q2 >> query;
    	
    	res = nodes_val.back()->eval(q1, q2);
    	if (query[0] == 't')
    		qc = 1;
    	else
    		qc = 0;
    	
    	cout << (((res%2==qc)||(res/2==qc))? 'Y' : 'N');
    }
    
}
```
{% endcode %}
{% endtab %}

{% tab title="Sol3" %}
문자열 입력 받을 때 O(N), 쿼리 해결 시 O(Q log N)

핵심 아이디어는 or(x, y, z, w, ...) 와 and(x, y, z, w, ...) 가 두 단계를 이루고 있단 점을 활용

* or(T, and(T, F, F, F, T), and(T, F), F, and(F, F, T), ...)



string 을 'or' seperator 로 분리 (string 양옆에도 or 하나씩 추가)

* or ... or ... or ... or ... or ... or
* 각 or 항 사이 시퀀스 값이 참인 개수를 누적합 방식으로 셈
* 시퀀스   값이 참인지 구분하는 법: (문자열 길이)/2+1 == (참 개수)&#x20;



query 구현

* (s, e) 를 포함하는 or ... or 시퀀스의 인덱스를 upper\_bound 를 통해 찾아냄 (= O(log N))
* 인덱스 (o1, o2) 내 문자열이 거짓이 되어도 전체 시퀀스는 참이라면 "True" 쿼리만 참
* (o1, o2) 내 참의 개수를 누적합을 통해 잘 카운트해서 (s, e) 제외한 참개수 +1 로도 (o1, o2) 가  참이 되는지 확인 -> 되면 "True" 쿼리, "False" 쿼리 둘 다 참

{% code lineNumbers="true" %}
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
	cin.tie(0)->sync_with_stdio(0);
	int N,Q, ors_n;
	
	bool val[200004] = {0};
	bool is_and[200004] = {0};
	int sum[200004] = {0};
	
	vector<int> ors;
	
	cin >> N >> Q;
	ors.push_back(0);
    ors_n++;
	for (int i=1;i<=N;i++) {
		string s;
		cin >> s;
		
		if (s[0] == 'a') {							// and
			is_and[i] = 1;
			sum[i] = sum[i-1];
		} else if (s[0] == 'o')	{			// or
			int old_o = ors[ors_n-1];
            is_and[i] = 0;
			ors.push_back(i);
            sum[i] =  (sum[i-1] == (i-old_o)/2) + sum[old_o];
            ors_n++;
		} else {
			is_and[i] = 1;
			val[i] = s[0] == 't';
			
			if (is_and[i-1])
				sum[i] = sum[i-2] + val[i];
			else
				sum[i] = val[i];
		}
	}
	ors.push_back(N+1);
    sum[N+1] =  (sum[N] == (N+1-ors[ors_n-1])/2) + sum[ors[ors_n-1]];
    ors_n++;
	
	while (Q--) {
		int s, e, o1, o2; 
		bool truestart=0, falsestart=0;
		string query;
		cin >> s >> e >> query;
        
        o1 = ors[upper_bound(ors.begin(), ors.end(), s) - ors.begin() - 1];
        o2 = ors[upper_bound(ors.begin(), ors.end(), e) - ors.begin()];

        if (sum[N+1] - sum[o2] + sum[o1] > 0)
        	truestart = falsestart = 1;
        else
        	truestart = ((o2-o1)/2 - (e-s)/2 ==  sum[o2-1] - (sum[e] - (is_and[s-1]?sum[s-1]:0)) + 1);

		
		if (query[0] == 't')
			cout << ((truestart || falsestart)? 'Y' : 'N');
		else 
			cout << ((!truestart || !falsestart)? 'Y' : 'N');
		
	}
	return 0;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}



{% embed url="https://www.youtube.com/watch?v=_8zjRCRpilk" %}

sol2 에서도 timeout 걸려서 구글링해서 solution 찾아봄

\-> 아이디어를 완전히 이해하지는 못했지만, 문자열 선형 탐색으로 구현해야겠다고 생각하여 sol3 구현, 성공
