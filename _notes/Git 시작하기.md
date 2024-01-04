
1. 현재 위치한 폴더를 git 폴더로 적용한다

``git init``

![Alt text](image/1.jpg)
숨겨진 폴더로 .git 폴더가 생성됨을 볼 수 있다.
git 폴더로 지정된 것이다.



2. 형상 관리할 파일을 생성한다. 

``touch 파일명.확장자``

![Alt text](image/2.jpg)
test1.txt 파일을 생성하였다.



3. git 폴더에 변경이 생긴 경우, git은 이를 감지한다

``git status``

![Alt text](image/3.jpg)

위에서 생성한 test1.txt 파일이 변경되었음을 감지했다.



4. 변경사항을 index 영역에 기록한다.

``git add .``


5. index 영역에 기록한 내용을 영구적으로 헤더영역에 기록한다.

``git commit -m "커밋메시지"``


6. 로그를 통해 작업이 잘 이루어졌는지 확인한다.

``git log``

![Alt text](image/4.jpg)



***
* HEAD가 가리키는 인덱스의 참조 해쉬값은 \\.git\\refs\\heads\\master 파일에서 확인 가능하다

![Alt text](image/5.jpg)

master 파일의 해쉬값이 git log에 기록된 HEAD가 가리키는 해쉬값과 같음을 확인할 수 있다.