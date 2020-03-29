---
layout : post
category: CodingTest
title: KAKAO 2019 겨울 인턴 모의고사
tags : [KAKAO,Programmers]
---

#### KAKAO 2019 겨울 인턴 모의고사 Review

- 오늘 프로그래머스 사이트에서 작년 겨울 인턴 코딩테스트를 모의고사 형태로 실시하였습니다.
- 해당 모의고사는 작년 시험과 같이 5문제를 4시간 안에 구현하도록 되어있습니다.

#### 1번

#### 문제 해석
  
- NxN격자에 인형들이 쌓여있고, 정해진 위치의 인형들을 뽑았을 때, 지울 수 있는 인형의 개수를 구하는 문제입니다.
- 정해진 열에 있는 가장 위의 인형을 뽑아서 장바구니에 담아둡니다.
- 장바구니에 같은 인형이 두개 있을 경우, 두개의 인형을 지웁니다.

#### 설계(5)

- 완전탐색문제입니다.
- 각 열에 있는 인형을 벡터에 미리 저장해놓습니다.
- 벡터의 연산중에 back연산을 이용하여 가장 위의 인형을 뽑아낸 후, 장바구니의 가장 위에 있는 인형과 비교하여 같을 경우 2를 더해주는 식으로 구현하였습니다.

#### 구현(5)

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

#### 2번

#### 문제 해석
  
- 중복되는 

#### 설계(5)

- 완전탐색문제입니다.
- 각 열에 있는 인형을 벡터에 미리 저장해놓습니다.
- 벡터의 연산중에 back연산을 이용하여 가장 위의 인형을 뽑아낸 후, 장바구니의 가장 위에 있는 인형과 비교하여 같을 경우 2를 더해주는 식으로 구현하였습니다.

#### 구현(5)

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
##### 디버깅

##### 제출결과


#### 마무리
