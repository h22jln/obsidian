- push
	현재 파일 저장소에 올리고자 하는 브랜치가 없더라도 올릴 수 있음
	``git push origin 브랜치명``
	
	 *모든 브랜치를 push 하고 싶다면*
	 ``git push --all``
	 ==repository 최초 생성시에만 사용하는 것이 좋다==
	
	 브랜치를 push후 PR 승인된 후에는 Github에서 branch를 삭제하는 것이 좋다
	 ``git push --delete origin 브랜치명``

- pull
	pull 하는 방법은 3가지가 있음
	1. git checkout -b topic(브랜치명)
		git fetch origin
		git merge origin/topic
		
	2. git checkout -b topic
		git pull origin topic -> *topic 브랜치 '만' 다운로드 및 머지*
		
	3. git fetch origin -> *모든 브랜치 다운로드*
		git checkout -b topic origin/topic -> *브랜치 생성 및 머지*

