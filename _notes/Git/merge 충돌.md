두 개의 branch가 같은 부분을 수정한 경우 [[merge#^d2c0b9||merge]]시 충돌나게 된다.


master branch에서 하나의 파일을 만든다
![Alt text](image/47.jpg)

새로운 branch를 하나 만들고 이 branch에서는 파일 내용을 수정한다
![Alt text](image/48.jpg)


다시 master로 돌아와서 이전 branch와 다른 내용으로 수정한다
![Alt text](image/49.jpg)



이 상태에서 merge를 하려고 하면
``git merge topic``

![Alt text](image/50.jpg)

충돌이 나게 되고
파일을 다시 열어보면

![Alt text](image/51.jpg)
충돌난 부분을 확인할 수 있다


둘 중에 사용하고자 하는 부분을 선택하여 수정하고,
다시 add > commit 하면 된다.