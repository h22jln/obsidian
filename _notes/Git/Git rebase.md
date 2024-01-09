여러 사람이 여러 브랜치로 작업한 후, 각각 Master로 merge하려고 하는 경우
==브랜치 checkout 순서대로== commit 로그가 생성되므로 
작업 순서와 상관 없이 로그가 쌓일 수 있다.

*ex)* 
첫 번째로 체크아웃한 브랜치 b1과 그 다음으로 체크아웃한 브랜치 b2가 있다.
b2가 먼저 master에 push하고 그 다음 b1이 push한 경우

```
b1 merge
b2 merge
b2 push
b1 push
init
```
이런식으로 push 순서와 상관없이 로그가 생성될 수 있다.

이를 방지하기 위해서는 b2를 merge한 후에 b1에서 master를 rebase해야한다.

``git rebase master``

rebase하게 되면 b1에 b2로그가 시간순으로 정상적으로 들어오게 되고
그 후에 commit push merge 하면 된다