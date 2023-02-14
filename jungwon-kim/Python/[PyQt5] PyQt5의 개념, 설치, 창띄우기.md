### ### PyQt5 의 개념과 창 띄우기

요즈음 만들고 싶은 게 많아졌다.

천천히 올해 안으론 끝내보자고 시작하는 프로그램 !

그러기 위해선 PyQt5 에 대해서 알아야 하므로 기록해본다.

## PyQt5 란?

![](https://wikidocs.net/images/page/21849/0_pyqt_logo.png)

- PyQt는 Python + Qt를 합쳐서 지은 이름으로, C++ 기반의 GUI Framework인 Qt를 Python에서 사용할 수 있게 만든 패키지
- 1000여 개의 클래스들을 포함하는 파이썬 모듈의 모음이다.
- Qt Designer에서 Python 코드를 생성
- Python으로 작성된 새로운 GUI 컨트롤을 Qt Designer에 추가하는 것도 가능
- Qt와 Python의 모든 장점을 결합 ( = Qt의 모든 기능을 가지고 있지만 Python으로 단순하게 활용 가능 )
- PyQt5는 윈도우, 리눅스, macOS, 안드로이드, iOS 지원
- [공식 문서 바로가기](https://www.riverbankcomputing.com/software/pyqt/intro)



## 설치 후 창 띄워보기

터미널에서 아래와 같이 입력 후 pyqt5를 설치한다.

파이썬이 미리 설치되어 있어야 한다.

``` python
pip install pyqt5
```



설치가 완료되면 아래 코드를 입력한다.

``` python
import sys
from PyQt5.QtWidgets import QApplication, QWidget

class MyApp(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setWindowTitle('My First Application')
        self.move(300, 300)
        self.resize(400, 200)
        self.show()

if __name__ == '__main__':
   app = QApplication(sys.argv)
   ex = MyApp()
   sys.exit(app.exec_())
```

- **setWindowTitle()** 메서드는 타이틀바에 나타나는 창의 제목을 설정함
- **move()** 메서드는 위젯을 스크린의 x=300px, y=300px의 위치로 이동함
- **resize()** 메서드는 위젯의 크기를 너비 400px, 높이 200px로 조절함
- **show()** 메서드는 위젯을 스크린에 보여줌

- **`__name__`**은 현재 모듈의 이름이 저장되는 내장 변수
  - 만약 'moduleA.py'라는 코드를 import해서 예제 코드를 수행하면 **`__name__`** 은 'moduleA'가 됨
  - 그렇지 않고 코드를 직접 실행한다면 `__name__` 은 `__main__` 
  - 따라서 이 한 줄의 코드를 통해 프로그램이 직접 실행되는지 혹은 모듈을 통해 실행되는지를 확인함

- app = QApplication(sys.argv)
  - 모든 PyQt5 어플리케이션은 어플리케이션 객체를 생성해야 한다.



이 후 실행 한다면 기본적인 창 띄우기 성공!

![](https://wikidocs.net/images/page/21920/2_1_opening.png)
