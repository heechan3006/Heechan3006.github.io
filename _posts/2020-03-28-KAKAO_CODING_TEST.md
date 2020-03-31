---
layout : post
category: CodingTest
title: KAKAO 2019 겨울 인턴 모의고사
tags : [KAKAO,Programmers]
---

#### KAKAO 2019 겨울 인턴 모의고사 Review

- 오늘 프로그래머스 사이트에서 작년 겨울 인턴 코딩테스트를 모의고사 형태로 실시하였습니다.
- 해당 모의고사는 작년 시험과 같이 5문제를 4시간 안에 구현하도록 되어있습니다.
- 사실 작년에 해당 인턴 테스트를 봤었는데 그 당시에 제대로 풀지 못했던 것들을 복기하기 위해서 다시 테스트를 해보았습니다.
- 원래 봤던 모의고사를 치르면서도 4번문제의 경우에는 풀이하는데 시간이 오래걸렸습니다.
- 개인적으로 문제가 엄청 어려운 편은 아니지만 약간의 고급 알고리즘이 필요한 문제가 있습니다. 카카오 문제다운 빡센 구현문제가 없어서 약간 아쉬운 면이 있습니다.
- 1,2,3번문제는 정확성만 보는 구현문제로, 완전탐색문제입니다.
- 4번은 아마 해당 모의고사 문제중 가장 어려운 문제가 아닐까 싶은데, 아이디어 자체를 생각하기 어려운 문제였습니다.
- 5번은 고급 알고리즘의 구현이 필요한 문제였습니다.

##### 1번 크레인 인형뽑기 게임

##### 문제 해석
  
- NxN격자에 인형들이 쌓여있고, 정해진 위치의 인형들을 뽑았을 때, 지울 수 있는 인형의 개수를 구하는 문제입니다.
- 정해진 열에 있는 가장 위의 인형을 뽑아서 장바구니에 담아둡니다.
- 장바구니에 같은 인형이 두개 있을 경우, 두개의 인형을 지웁니다.

##### 설계(5)

- 완전탐색문제입니다.
- 각 열에 있는 인형을 아래에서부터 순서대로 벡터에 미리 저장해놓습니다.
- 벡터의 연산중에 back연산을 이용하여 가장 위의 인형을 뽑아낸 후, 장바구니의 가장 위에 있는 인형과 비교하여 같을 경우 2를 더해주는 식으로 구현하였습니다.

##### 구현(5)

##### 코드

```cpp

#include <iostream>
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> board, vector<int> moves) {
    int answer = 0;
    int R = board.size();
    int C = board[0].size();
    vector<int> ans;
    vector<vector<int> > arr(C);
    for (int j = 0; j < C; j++){
        int i = R-1;
        while (i>=0 && board[i][j]!=0){
            arr[j].push_back(board[i][j]);
            i--;
        }
    }
    for (int i = 0; i < moves.size(); i++){
        int idx = moves[i] - 1;
        if (!arr[idx].size()) continue;
        if (ans.size()>0 && ans.back() == arr[idx].back()) {
            answer+=2;
            ans.pop_back();
        }
        else{
            ans.push_back(arr[idx].back());
        }
        arr[idx].pop_back();
    }
    return answer;

}
```

##### 2번 튜플

##### 문제 해석
  
- 중복되는 원소가 없는 집합이 주어질 때, 해당 집합이 나타내는 튜플을 찾는 문제입니다.
- 이때, 집합은 원소의 순서가 바뀌어도 상관없습니다.

##### 설계(10)

- 정렬 + 완전 탐색문제입니다.
- 먼저, 주어지는 입력은 string형태로, 원소의 집합만 나오게 파싱을 해야합니다.
- 적절히 파싱한 데이터를 벡터에 저장합니다.
- 다음으로, 알맞은 튜플을 찾기 위해서는 원소의 개수가 적은 순서대로 정렬을 진행합니다.
- 정렬이 되면 각 인덱스마다 아직 나오지 않은 원소가 딱 한개씩만 남게 됩니다.
- 튜플로 이미 사용된 원소는 visited배열을 체크하면서 구분하게 합니다.
- 정렬하는데 O(NlogN) + 튜플을 찾는데 O(N<sup>2</sup>)의 시간복잡도가 생깁니다.

##### 구현(20)

##### 코드

```cpp

#include <string>
#include <vector>
#include <algorithm>
#define MAXN 100001
using namespace std;
bool comp(vector<int> a, vector<int> b){
    return a.size() < b.size();
}
bool visited[MAXN];
vector<int> solution(string s) {
    vector<int> answer;
    vector<vector<int> > num;
    for (int i = 0;i< s.length(); i++){
        if (s[i]>= '0' && s[i] <='9'){
            vector<int> tmp;
            bool flag = false;
            while (!flag){
                int tmp_num = 0;
                while (1){
                    if (s[i] == '}') {
                        tmp.push_back(tmp_num);
                        flag = true;
                        break;
                    }
                    else if (s[i] == ',') {
                        tmp.push_back(tmp_num);
                        tmp_num = 0;
                    }
                    else{
                        tmp_num *= 10;
                        tmp_num += s[i] -'0';
                    }
                    i++;
                }
            }
            num.push_back(tmp);
        }
    }
    sort(num.begin(),num.end(),comp);
    for (int i = 0;i < num.size(); i++){
        for (int j =0 ;j < num[i].size(); j++){
            if (!visited[num[i][j]]){
                visited[num[i][j]] = true;
                answer.push_back(num[i][j]);
            }
        }
    }
    return answer;
}
```

##### 3번 불량사용자

##### 문제 해석
  
- 이벤트 응모자 아이디 목록과 불량 사용자 아이디 목록이 주어졌을 때, 당첨에서 제외되어야할 제재 아이디 목록은 몇가지 경우의 수가 가능한지 구하는 문제입니다.
- 불량사용하 아이디는 하나이상의 "*"를 포함하고 있으며, 해당 위치에 모든 알파벳과 소문자가 나올 수 있습니다.
- 제재 아이디 목록들을 구했을 때, 아이디들이 나열된 순서와 관계없이 아이디 목록의 내용이 동일하다면 같은것으로 처리하여 하나로 세면 됩니다.

##### 설계(5)

- 완전탐색과 set자료구조를 활용하는 문제입니다.
- 제재 아이디에 들어가는 모든 응모자아이디의 경우를 구하기 위해서 O(N<sup>N</sup>)의 시간복잡도가 필요합니다. 최악의 경우에 8<sup>8</sup>=약 1.67x10<sup>7</sup>으로 구현이 가능합니다.
- check함수로 현재 응모자 아이디와 제재 아이디를 비교하여 가능한 경우 used배열을 이용하여 체크/해제를 합니다.
- 이와 동시에 벡터에 해당 응모자 아이디를 저장해놓습니다.
- 제재아이디에 모두 포함되는 경우, set에 해당 벡터를 저장하여 중복되는 조합이 카운트되는 것을 방지합니다.(순서를 일치시키기 위해서 정렬을 먼저 해야합니다)

##### 구현(15)

##### 코드

```cpp
#include <string>
#include <vector>
#include <set>
#include <algorithm>

using namespace std;

int answer;
set<vector<string> > st;
vector<bool> used;
vector<string> vt;
bool check(string user, string ban){
    if (user.size() != ban.size()) return false;
    for (int i = 0;i < user.length(); i++){
        if (ban[i]!='*' && user[i] != ban[i]) return false;
    }
    return true;
}
void dfs(int cnt, vector<string> user_id, vector<string> banned_id){
    if (cnt == banned_id.size()){
        vector<string> tmp_v = vt;
        sort(tmp_v.begin(),tmp_v.end());
        if (st.count(tmp_v)==0){
            st.insert(tmp_v);
            answer++;
        }
        return;
    }
    for (int i = 0;i < user_id.size(); i++){
        if (!used[i] && check(user_id[i],banned_id[cnt])){
            used[i] = true;
            vt.push_back(user_id[i]);
            dfs(cnt+1,user_id,banned_id);
            used[i] = false;
            vt.pop_back();
        }
    }
}
int solution(vector<string> user_id, vector<string> banned_id) {
    answer = 0;
    used = vector<bool>(user_id.size(),false);
    dfs(0,user_id,banned_id);
    return answer;
}

```

##### 4번 호텔 방 배정

##### 문제 해석
  
- 다음 아래의 규칙에 따라 고객에게 방을 배정하는 문제입니다.
   1. 한 번에 한명씩 신청한 순서대로 방을 배정합니다.
   2. 고객은 투숙하기 원하는 방 번호를 제출합니다.
   3. 고객이 원하는 방이 비어있다면 즉시 배정합니다.
   4. 고객이 원하는 방이 이미 배정되어있으면, 원하는 방보다 크면서 **비어있는 방 중 가장 번호가 작은 방**을 배정합니다.

##### 설계(140)

- 이 문제는 아이디어를 생각하기가 정말 어려웠던 문제입니다.
- 언뜻 보기에 이분 탐색이나 DP 또는 세그먼트 트리를 이용하면 될 것 같았지만 그리 쉽지 않았습니다.
- 오랜 고심 끝에 union-find를 이용하여 구현하였습니다.
- 모든 방을 선택하면서 해당 방의 -1,+1번째 방과 연결되어있는지 체크를 한 후, 큰 숫자가 작은 숫자의 부모가 되게 이어줍니다.
- 숫자의 범위가 10<sup>12</sup>이하이기 때문에 직접 배열로 표현을 할 수가 없습니다. 이를 대처하기 위해서 map자료구조를 사용합니다.
- 만약 원하는 방이 비어있으면 그냥 map에 저장합니다.
- 그렇지 않은 경우에 find함수로 연결되어있는 노드의 최상단 노드보다 1이 큰 방을 지정해주면 됩니다.

##### 구현(20)


```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <map>
using namespace std;
typedef long long ll;

map<ll,ll> mp;
ll find(ll x){
    if (mp.count(x) ==0) return -1;
    if (mp[x] == x) return x;
    return mp[x] = find(mp[x]);
};
void Merge(ll a, ll b){
    a = find(a);
    b = find(b);
    if (a < b){
        mp[a] = b;
    }
    else if (a > b){
        mp[b] = a;
    }
}
vector<long long> solution(long long k, vector<long long> room_number) {
    vector<long long> answer;
    for (int i = 0 ;i < room_number.size(); i++){
        ll target = room_number[i];
        if (mp.count(room_number[i])==0){
            mp[target] = target;
            answer.push_back(target);
        }
        else{
            target = find(target)+1;
            mp[target] = target;
            answer.push_back(target);
        }
        ll a = find(target-1);
        ll b = find(target+1);
        if (a!= -1 && a < target){
            mp[a] = target;
        }
        if (b != -1 && target < b){
            mp[target] = b;
        }
    }
    return answer;
}

```

##### 5번 징검다리 건너기

##### 문제 해석
  
- 다음의 규칙을 만족하는 최대로 징검다리를 건널수 있는 사람 수를 구하는 문제입니다.
   1. 징검다리는 일렬로 놓여있고, 모두 숫자가 적혀있으며 한번 밟을때마다 1씩 줄어듭니다.
   2. 디딤돌의 숫자가 0이 되면 더이상 밟을 수 없으며, 그 다음 디딤돌로 한번에 여러 칸을 건너 뛸 수 있습니다.
   3. 다음으로 밟을 수 있는 디딤돌이 여러개인 경우 무조건 가장 가까운 디딤돌로만 건너 뛸 수 있습니다.
- 한번에 건너 뛸 수 있는 디딤돌의 최대 칸수 K가 주어집니다.

##### 설계(20)

- 이분탐색 + 세그먼트트리를 이용하는 문제입니다.
- 먼저 완전탐색으로 구현하면 O(N<sup>2</sup>)으로 효율성은 통과하지 못할 것입니다.
- 이분탐색으로 건널 수 있는 사람 수를 구하고, 세그먼트 트리를 이용하여 구간 최대값을 이용하면 O(N(logN)<sup>2</sup>)으로 효율성이 통과될 것으로 보입니다.
- 먼저 세그먼트 트리를 생성합니다.
- 다음으로 최소로 건널 수 있는 숫자 0에서 최대로 건널수있는 숫자 2x10^8까지 이분탐색으로 건널 수 있는 숫자를 정합니다. O(logN)
- 모든 N에 대해서 구간 (x,x+K)의 최대값이 건널 수 있는 숫자보다 작지 않은 지 체크하여 건널 수 있는지 없는지 판단하게 합니다. O(NlogN)

##### 구현(50)

```cpp
#include <string>
#include <vector>
#include <math.h>

using namespace std;
vector<int> tree;

int init(vector<int>& tree, vector<int>& arr, int node, int start, int end){
    if (start == end) return tree[node] = arr[start];
    int mid = (start+end)/2;
    return tree[node] = max(init(tree,arr,node*2,start,mid),init(tree,arr,node*2+1,mid+1,end));
}

int calc(int node, int start, int end, int left, int right){
    if (left > end || right < start) return 0;
    if (left <= start && end <= right) return tree[node];
    int mid = (start + end) /2;
    return max(calc(node*2,start,mid,left,right),calc(node*2+1,mid+1,end,left,right));
}
bool possible(int num,vector<int> stones,int k){
    for (int i = 0 ;i <= stones.size()- k; i++){
        int max_value = calc(1,0,stones.size()-1,i,i+k-1);
        if (max_value < num) return false;
    }
    return true;
}
int solution(vector<int> stones, int k) {
    int answer = 0;
    int N = stones.size();
    int h = (int)ceil(log2(N));
    int tree_size = (1<<(h+1));
    tree = vector<int>(tree_size);
    init(tree,stones,1,0,N-1);
    int low = 0;
    int high = 2*1E9;
    while (low <= high){
        int mid = (low + high) / 2;
        if (possible(mid,stones,k)){
            low = mid + 1;
            answer = max(answer,mid);
        }
        else{
            high = mid - 1;
        }
    }
    return answer;
}

```

#### 마무리
