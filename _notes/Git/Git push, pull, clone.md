##### local 에서 origin으로 파일을 업로드 하려면?

1. Github repository 생성
2. local에서 add > commit 
3. origin과 local 연결
	``git remote add repository_주소``
	
	*origin과 잘 연결되었나 확인하려면*
	``git remote -v`` or ``git ls-remote``
	
4. origin에 local 파일 업로드
	``git push origin master``
	
	*origin도 일종의 branch라고 할 수 있다*
	*branch끼리 파일을 합치려면 merge를 하는데*
	*파일업로드와 merge를 한번에 하는 명령어가* ``push``


##### origin에서 local로 파일을 가져오려면?

1. local과 origin 연결
	``git remote add repository_주소``
	
	*잘못 연결되었다면 연결 삭제 가능*
	``git remote rm origin``
	
2. origin에서 파일 땡겨오기
	``git pull origin master``


##### clone
git init + git remote + git pull = ==git clone==
``git clone repository_주소`` 로 origin에서 가져오면 
repository 이름으로 된 폴더가 생기고
폴더 내부로 들어가서 log를 확인 가능하다
