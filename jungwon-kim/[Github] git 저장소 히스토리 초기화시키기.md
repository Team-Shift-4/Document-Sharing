프로젝트를 진행할 때, gitignore를 미리 작성하지 않아서 내 api key가 올라가버렸다.

뒤늦게 지워보았지만 남아있는 기록들😱

그래서 히스토리를 초기화하려고 방법을 알아봤다!



***



먼저, 해당 프로젝트 폴더에서 gitbash를 열어주고, 아래와 같이 실행하면 된다.



1. 로컬 저장소의 git 히스토리 삭제

```
$ rm -rf .git
```

 

2. 로컬 저장소를 다시 초기화

```
$ git init
```

 

3. 초기화할 파일을 추가하고 커밋하기

```
$ git add .
$ git commit -m "project commit"
```

 

4. 저장소 연결 

```
$ git remote add origin <url>
```

해당 레포지토리에 들어가 Code 버튼을 눌러 Clone 칸에 있는 HTTPS url 을 사용하면 된다.



5. push

```
$ git push -u --force origin master
```



master로 push를 해준 뒤, 들어가서 디폴트 브랜치를 master로 바꿔주면 된다.





이렇게 하면 깔끔해진 히스토리를 볼 수 있다.

잔디에 구멍이 난 건 슬프지만 내 실력 부족이였던거니까...🥲 앞으론 다시 이런 일 반복하지 말자!