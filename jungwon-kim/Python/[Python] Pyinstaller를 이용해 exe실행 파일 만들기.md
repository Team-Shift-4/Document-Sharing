## Pyinstaller

코딩한 파이썬 프로그램을 실행 파일(.exe)로 만들어주는 파이썬 패키지이다.



👉 설치 방법

터미널 창에서 실행

```python
# 설치
pip install pyinstaller

# 최신 버전으로 업그레이드 (첫 설치 시에는 필요없음)
pip install --upgrade pyinstaller

# 설치되어 있는지 확인
pyinstaller --version
```

 

***

 

### 👉 사용 방법

터미널 창에서 실행!

``` python
# 가장 기본적인 사용법
pyinstaller file.py
```

 

### 📌 여러가지 옵션들

``` python
pyinstaller -F -w file.py
```



**1) -F, --onefile**

실행파일 1개만 생성되도록 해주는 설정이다.

> 참고) **실행 속도 향상을 위한 팁**
>
> --onefile 옵션으로 단 한 개의 blog_tkinter.exe 파일을 생성할 때 편리하긴 하지만 실행이 느리다는 단점이 있다. 컴퓨터 사양에 따라 속도 차이는 있겠지만, 실행을 위해 대략 5초 이상의 시간이 걸린다.
>
> 실행 속도를 빠르게 하려면 --onefile 옵션을 생략하고 pyinstaller를 실행하면 된다. 이때는 blog_tkinter.exe 파일 외에 많은 부수적인 파일이 생성되므로 배포 시에 blog_tkinter.exe 파일 한 개가 아닌 dist 디렉터리에 생성된 모든 파일을 zip 등으로 압축하여 전달하든가 별도의 설치 파일 제작 프로그램(inno setup 또는 nsis 등)을 이용하여 설치 파일로 만든 다음 제공해야 한다.
>
> [참고 사이트](https://wikidocs.net/133214)

 

**2) -w, --windowed, --noconsole**

기본적으로 실행 파일을 실행하면 표준 I/O용 콘솔 창(까만 창)을 열도록 되어있는데 이걸 뜨지 않도록 하는 옵션이다.

 

**3) -N, -n**

파이썬 파일명으로 프로그램이 만들어지는데,  이 옵션을 사용하면 프로그램 이름을 지정해줄 수 있다.

```python
pyinstaller -F -w -n 지정할프로그램명 file.py
```

 

**4) --icon**

`--icon=` 뒤에 아이콘 경로를 적어주면 프로그램의 아이콘으로 설정할 수 있다. 

```python
pyinstaller --icon=./test.ico file.py
```

***

 

실행하고 나면 현재 경로에 dist 폴더에 exe 파일이 생긴 것을 확인할  수 있다.

이 파일을 사용하는 사람들은 다른 모듈을 다운해서 사용할 필요는 없다!

자 이제 pyinstaller 를 이용해서 쉽게 프로그램을 만들어보자😊 

 



[공식 사이트 바로가기](https://pyinstaller.org/en/stable/)