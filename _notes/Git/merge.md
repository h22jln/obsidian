시나리오) main 브랜치에서 1, 2 두 번의 commit이 일어난 상태
이 때 새로운 브랜치를 생성하여 3 이라는 commit을 했다.

- fast-forward-merge
	새로운 브랜치에서 만든 3 을 main branch로 합치고 싶은 경우
	main 브랜치의 HEAD가 새로운 브랜치의 마지막 점인 3을 가리키면 된다.

- 3-way-merge
	새로운 브랜치의 3 commit 후에 main 브랜치에서 4 새로운 commit이 발생한 경우, fast-forward 방식으로 merge하게 되면 4 지점의 작업은 날아가게 된다.
	
	이 때는 3과 4중 어느 브랜치로 merge 할지 결정하여 main의 HEAD를 옮기면 된다. ^d2c0b9

그러나 3과 4가 같은 파일을 수정하여 merge시 충돌하는 경우가 생길 수 있다.

이때는 auto merge가 안 되므로 개발자가 직접 conflict를 수정하여 commit 해야한다.

***
##### ==fast-forward-merge== 실습

![Alt text](image/40.jpg)
master, topic 두개의 branch가 있다


![Alt text](image/41.jpg)
master branch가 가리키고 있는 시점 이후에 topic branch에서 새로운 commit을 했다.

![Alt text](image/42.jpg)
master branch로 이동 후
``git merge topic(합치고자 하는 브랜치명)`` 입력하면 
master와 topic branch가 merge 되었음을 확인 가능하다.

***

##### ==3-way-merge== 실습

![Alt text](image/43.jpg)
master branch는 로그인 이후 시점에 글쓰기 라는 것이 생겼고


![Alt text](image/44.jpg)
topic branch는 로그인 이후에 아이디중복체크라는 것이 생겼다.


이전과 동일하게 master branch에서 
``git merge topic`` 입력시

![Alt text](image/45.jpg)
vi 에디터가 나오게 되는데

*노란색이 커밋메시지이다*
수정하려면 ``i``로 입력모드 진입하면 된다.
``:wq``로 빠져나오면 merge가 완료된다.

![Alt text](image/46.jpg)








