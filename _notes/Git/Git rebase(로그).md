![커밋 로그 이미지](image/13.jpg)
이 네개의 커밋 로그들을 요리조리 실습해본다

***

``git rebase -i HEAD~3``
HEAD 뒤의 숫자는 HEAD를 기준으로 과거 몇 개를 #rebase 할건지에 관한 값


![rebase vi 에디터](image/14.jpg)

rebase로 접근하면 vi에디터가 나온다
``i``를 입력하면 INSERT MODE 접근 가능


![rebase vi 에디터](image/15.jpg)

맨 위의 하나만 남기고 ``d``로 바꿔주었다.
esc > :wq 로 저장후 빠져나온다.



![Alt text](image/16.jpg)

``git log``로 확인해보니 ``d``로 설정한 로그들(파일도 같이)이 완전 삭제되었다



삭제된 파일을 되살리고 싶다면
[[Git reset#^210a29||git reset --hard]]로 되살린다.

![Alt text](image/17.jpg)

``git reset --hard c38cae3``

![Alt text](image/18.jpg)
로그 뿐 아니라 파일도 되돌아왔다.


***

squash의 원리)
1, 2, 3 로그가 있을 때 파일은 3(최신버전)을 가져가고, 로그는 1(남기고자 하는 버전)을 유지하여 로그만 지워지는 것 처럼 보이게 한다

***


``git rebase -i HEAD~3``


![Alt text](image/19.jpg)

vi 에디터 접근 후, ``i`` 입력하여 입력모드로 변경

![Alt text](image/20.jpg)

pick 하나만 두고 나머지는 s(squash)로 지정
==pick은 가장 과거의 로그로 지정할 것==


esc > :wq로 빠져나오면

![Alt text](image/21.jpg)

또 다른 에디터가 나오게 되고,
입력 모드로 변환하여 #3 번째의 로그를 작성하면 된다.

![Alt text](image/22.jpg)

로그가 정리됨을 확인할 수 있다.

***

rebase option
- r(reword) : 커밋 메시지 내용 수정
* p(pick)에서 commit이 일어나게 됨
* d(drop) : 커밋 로그 및 파일을 아예 삭제

