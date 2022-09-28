# Rebase

Rebase는 Branch의 Base를 변경한다는 개념이다.
Rebase는 Branch간에 Base가 달라야 실행 가능하다.

#### 1. 포함 관계인 경우

-   아래 예시와 같이 Base가 상대 branch의 커밋이 최신인 경우 (포함 관계)는 Base를 재조정 할 필요가 없기 때문이다.

```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'A-branch'}} }%%
      gitGraph
        commit id: "A-1"
        commit id: "A-2" tag: "A-branch"
        branch B-branch
        commit id: "B-1"
        commit id: "B-2" tag: "B-branch"
```

#### 2. 포함 관계가 아닌 경우

-   Base가 다르기 때문에 실행 가능하다.

```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'A-branch'}} }%%
      gitGraph
        commit id: "A-1"
        commit id: "A-2"
        branch B-branch
        commit id: "B-1"
        commit id: "B-2" tag: "B-branch"
        checkout A-branch
        commit id: "A-3" tag: "A-branch"
```

```
git checkout B-branch
git rebase A-branch
// 아래 그림과 같이 목적(B) 커밋들을 주체(A) 최신 커밋으로 베이스를 변경한다.
```

```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'A-branch'}} }%%
      gitGraph
        commit id: "A-1"
        commit id: "A-2"
        commit id: "A-3" tag: "A-branch"
        branch B-branch
        commit id: "B-1"
        commit id: "B-2" tag: "B-branch"
```

```
git checkout A-branch
git merge B-branch
// fast-forward를 실행으로 상태를 같게 만든다.
```

```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'A-branch'}} }%%
      gitGraph
        commit id: "A-1"
        commit id: "A-2"
        commit id: "A-3"
        commit id: "B-1"
        commit id: "B-2" tag: "A-branch | B-branch"
```
