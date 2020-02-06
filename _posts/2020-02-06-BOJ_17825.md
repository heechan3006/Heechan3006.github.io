---
category: ProblemSolving
title: BOJ 17825
tags : [BOJ]
---
### 주사위윷놀이 
[문제출처](https://www.acmicpc.net/problem/17825)

#### 문제 해석
 - 주어진 10개의 주사위 숫자로 윷놀이를 했을 때, 얻을 수 있는 점수의 최댓값을 구하는 문제입니다.
 - 말이 4개가 있고, 1~5가 적혀있는 주사위 수만큼 이동이 가능합니다.
 - 말의 이동 경로는 출처의 그림과 같이 나와있으며, 파란원에 도착하면 경로가 파란 화살표로 바뀌게 됩니다.
 - 말이 도착칸 또는 도착칸보다 더 멀리 이동할 시에는 도착칸에 머물러 있으며, 더이상 이동할 수 없습니다.
 - 시작과 도착칸은 중복이 되어도 상관이 없고, 나머지 칸들의 경우에 다른 말이 있으면 해당 칸으로 이동할 수 없습니다.
 - 말이 이동을 마칠 때마다 칸에 적혀있는 점수가 추가됩니다.
      
#### 설계(25)
 - 해당 문제는 백트래킹으로 구현이 가능합니다. 첫번째로, 각 윷놀이판의 경로를 지정해놓습니다.
 - 기본적으로 경로 0부터 시작하여, 각 파란원에 도착하는 경우 그에 맞는 경로로 바뀌도록 설정해 놓습니다.
 - 모든 말에 대해 주사위를 굴려보고, 해당 칸에 말이 중복되지 않는 경우를 찾습니다.
 - 각 말의 벡터 인덱스와 경로를 저장해 놓아서 윷놀이판중 어느 위치에 있는지 파악합니다.
 - 여기서 주의할 점은, 말이 도착한 부분을 백트랙할 때 도착지역의 방문을 해제하는 일이 없도록 해야합니다.
    
#### 구현(35)

##### 코드
```cpp
#include <iostream>
#include <vector>
using namespace std;
struct node {
	int idx;
	int path_idx;
};
node horse[4];
bool visited[34];
vector<pair<int, int> > adj[4];
vector<pair<int, int> > tmp_v[4] = { { {21,0} },
									{ {22,13},{23,16},{24,19},{25,25},{31,30},{32,35},{20,40},{21,0} },
									{ {29,22},{30,24},{25,25},{31,30},{32,35},{20,40},{21,0} },
									{ {28,28},{27,27},{26,26},{25,25},{31,30},{32,35},{20,40},{21,0} } };
int ans = 0;
int dice[10];
node move(int i, int cnt) {
	int idx = horse[i].idx;
	int path_idx = horse[i].path_idx;
	idx += cnt;
	if (idx >= adj[path_idx].size()) idx = adj[path_idx].size() - 1;
	// 경로 1로 변경되는 경우
	if (adj[path_idx][idx].first == 5) {
		path_idx = 1;
	}
	// 경로 2
	else if (adj[path_idx][idx].first == 10) {
		path_idx = 2;
	}
	// 경로 3
	else if (adj[path_idx][idx].first == 15) {
		path_idx = 3;
	}

	if (adj[path_idx][idx].first != 21 && visited[adj[path_idx][idx].first]) return { -1,-1 };
	return { idx,path_idx };
}
void dfs(int dice_idx, int sum) {
	if (dice_idx == 10) {
		if (ans < sum) ans = sum;
		return;
	}
	for (int i = 0; i < 4; i++) {
		// 해당 말이 도착한 경우 제외
		if (adj[horse[i].path_idx][horse[i].idx].first == 21) continue;
		node tmp = move(i, dice[dice_idx]);
		if (tmp.idx == -1) continue;
		// 현재 위치, 다음 위치 갱신
		int next_idx = adj[tmp.path_idx][tmp.idx].first;
		int cur_idx = adj[horse[i].path_idx][horse[i].idx].first;
		int next_w = adj[tmp.path_idx][tmp.idx].second;
		visited[cur_idx] = false;
		visited[next_idx] = true;
		node tmp1 = horse[i];
		horse[i] = tmp;
		dfs(dice_idx + 1, sum + next_w);
		if (next_idx != 21) {
			visited[next_idx] = false;
		}
		visited[adj[tmp1.path_idx][tmp1.idx].first] = true;
		horse[i] = tmp1;
	}
}
int main() {
	ios::sync_with_stdio(false);
	cin.tie(0);
	for (int i = 0; i <= 20; i++) {
		adj[0].push_back({ i,i * 2 });
		if (i <= 5) {
			adj[1].push_back({ i,i * 2 });
		}
		if (i <= 10) {
			adj[2].push_back({ i,i * 2 });
		}
		if (i <= 15) {
			adj[3].push_back({ i,i * 2 });
		}
	}
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < tmp_v[i].size(); j++) {
			adj[i].push_back(tmp_v[i][j]);
		}
	}
	for (int i = 0; i < 10; i++) {
		cin >> dice[i];
	}
	dfs(0, 0);
	cout << ans << "\n";
	return 0;
}
```
##### 디버깅   
 - 탐색 후, 방문체크를 해제할 때 말의 정보를 이전 정보로 되돌리지 않은 실수를 하여 디버깅 시간이 오래걸렸습니다.
      
##### 제출결과
 - 8ms

#### 마무리
 - 아주 어려운 구현문제입니다. 각 경로를 따로 지정해주어야 하고, 여러모로 생각할 부분이 많아 시간이 오래 걸렸습니다.
