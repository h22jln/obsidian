#reset : 복구 명령어

- option
	1. #soft : HEAD가 가리키는 인덱스 참조값을 되돌린다
		(add 이후 시점으로 이동)
	2. #mixed : 파일 add 시점 이전으로 이동
	3. #hard : 파일을 만들기 이전 시점으로 이동


``git reset --option 돌아갈 인덱스의 해쉬값 일부``


- reset soft
	
	![Alt text](image/6.jpg)
	두 파일이 commit된 상태에서 reset soft
	
	``git reset --soft 09dbdc``
	
	![Alt text](image/7.jpg)
	git status로 확인 시 파일 변경이 없고 (= git add 이후 시점이라는 의미)
	git log로 확인 시 test commit 1 만이 남아있음을 확인할 수 있다
	
	==커밋 메시지를 수정할 때 사용가능한 기능이다.==
 ^39a59d

- reset mixed
	![Alt text](image/6.jpg)
	두 파일이 commit된 상태에서 reset mixed
	
	``git reset --mixed 09dbdc``
	
	![Alt text](image/8.jpg)
	reset mixed 이후 git status로 확인 시 파일이 untracked됨을 확인할 수 있다.
	
	==파일 내용을 수정하고자 할 때 사용 가능한 기능이다==


- reset hard
	![Alt text](image/6.jpg)
	두 파일이 commit된 상태에서 reset hard
	
	``git reset --hard 09dbdc``
	
	![Alt text](image/9.jpg)	
	test2.txt 파일 자체가 사라짐을 확인할 수 있다
	
	==작업 내용을 아예 지우고 처음부터 다시 하고자 할 때 사용가능한 기능이다==
 ^210a29


- git #reflog
	위의 과정들로 reset 되었더라도 commit된 모든 기록을 확인할 수 있다
	
	``git reflog``
	
	![Alt text](image/10.jpg)
	
	여기서 확인한 해쉬값을 사용하여 reset을 reset 할 수 있다!
	(hard option으로 파일까지 지워진 경우여도)
	
	``git reset --hard 해쉬값``
	
	![Alt text](image/11.jpg)
