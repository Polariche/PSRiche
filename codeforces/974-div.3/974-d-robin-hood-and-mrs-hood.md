---
description: '#slidingwindow #imos'
---

# 974 D - Robin Hood and Mrs Hood

{% embed url="https://codeforces.com/contest/2014/problem/D" %}

### Problem Statement

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p>Test case #3</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Sliding window on Test case #3</p></figcaption></figure>



### Sliding Window

Day 1 \~ N 까지 sweeping 하면서, size = D 인 윈도우와 겹치는 job 의 수를 센다

이를 위해선 입력 단계에서 각 day 마다 시작하는 job의 목록, 끝날 job 의 목록을 기록한다

실제 sweeping 단계에선 다음과 같이 윈도우와  겹치는 job 의 목록을 업데이트한다

* window 의 앞단 (= D\~N) 에서 시작되는 job -> set 에 insert
* window 의 뒷단 (= 1 \~ N-D+1) 에서 끝날 job -> set 에서 erase

job 목록 업데이트가 끝나면 job 수의 min, max (와 index) 를  업데이트한다



### Solution

{% code lineNumbers="true" %}
```cpp
#include <iostream>
#include <vector>
#include <set>
using namespace std;

pair<int,int> test(int T) {
	int N,D,K;
	cin >> N >> D >> K;
	
	vector<pair<int,int>> job(K);
	vector<set<int>> enterj(N+1);
	vector<set<int>> exitj(N+1);
	
	set<int> cur_jobs;
	
	int minjob=K+1, minjob_idx;
	int maxjob=0, maxjob_idx;
	
	
	for (int i=0;i<K;i++)
		cin >> job[i].first >> job[i].second;
		
	for (int i=0;i<K;i++) {
		auto [s,e] = job[i];
		
		enterj[s].insert(i);
		exitj[e].insert(i);
	}
	
	// sliding window init
	for (int i=0;i<D;i++) {
		for (int j : enterj[i])
			cur_jobs.insert(j);
	}
	
	// sliding window
	for (int i=D;i<=N;i++) {
		int size;
		
		for (int j : enterj[i])
			cur_jobs.insert(j);
		for (int j : exitj[i-D])
			cur_jobs.erase(j);
		
		size = cur_jobs.size();
		if (size > maxjob) {
			maxjob = size;
			maxjob_idx = i-D+1;
		}
		
		if (size < minjob) {
			minjob = size;
			minjob_idx = i-D+1;
		}
	}
		
	return {maxjob_idx, minjob_idx};
}

int main() {
	cin.tie(0)->sync_with_stdio(0);
	
	int T;
	cin >> T;
	while (T) {
		auto res = test(T--);
		cout << res.first << " " << res.second << "\n";
	}
	
	return 0;
}
```
{% endcode %}



### Further Readings

* imos법 : [https://driip.me/65d9b58c-bf02-44bf-8fba-54d394ed21e0](https://driip.me/65d9b58c-bf02-44bf-8fba-54d394ed21e0)
  * job 을 distinct 하게 관리하지 않고, enterj -> +1, exitj -> -1 로 세는 것도 가능\
    해당 구현이 set 을 사용할 때 보다 더욱 효율적이며, 공식 tutorial 코드([https://codeforces.com/blog/entry/134093](https://codeforces.com/blog/entry/134093))도 이와 같이 구현되어 있다&#x20;
