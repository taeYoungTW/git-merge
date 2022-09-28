# Rebase

Rebase는 Branch의 Base를 변경한다는 개념이다.
Rebase는 Branch간에 Base가 달라야 실행 가능하다.

정리

-   재정렬 기준 브랜치(`git rebase {baseBranch}`)의 커밋들 중에 주체 브랜치가 포함하고 있는 커밋은 그대로 연결하고, 포함하고 있지 않은 커밋은 재정렬 기준 브랜치의 최신 커밋 뒤에 연결한다.

#### 1. 포함 관계인 경우

-   아래 예시와 같이 Base가 상대 branch의 커밋이 최신인 경우 (포함 관계)는 Base를 재조정 할 필요가 없기 때문이다. (B에서 A로 rebase하는 경우)
-   다만, 해당 상황에서 A에서 B로 rebase하는 경우 A-branch에서 앞선 B-branch를 rebase하는 경우 fast-forward를 작동한 것과 같이 B-2로 A-branch,B-branch 모두 향한다.

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

#### 반대 방향

-   아래 같은 상황에서 A-branch에서 B-branch로 rebase하는 경우, 목적 Base인 B-2를 기준으로 붙여지게 된다.

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
git checkout A-branch
git rebase B-branch
```

```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'A-branch'}} }%%
      gitGraph
        commit id: "A-1"
        commit id: "A-2"
        commit id: "B-1"
        commit id: "B-2" tag: "B-branch"
        commit id: "A-3" tag: "A-branch"
```
