[[Git reset|reset soft]]와 다르지만 비슷한 기능을 한다.

git amend 옵션을 통해 커밋 메시지를 다시 쓸 수 있게 된다

``git commit --amend -m "메시지"``

![Alt text](image/12.jpg)


> [!NOTE]
> reset soft는 add 후의 시점으로 되돌아가지만, commit --amend는 메시지를 수정하고 바로 다시 커밋하므로 기능상 비슷하지만 완전히 다른 기능이라고 생각한다.
> 
> 아직 add후 바로 commit 밖에 하지 않지만 add와 commit 사이에 당연히 여러 액션이 존재하겠지... 유용할지 자주쓸지 어쩔지는 모르지만


