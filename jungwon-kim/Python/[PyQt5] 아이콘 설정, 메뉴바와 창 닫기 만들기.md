## 창의 아이콘 넣기

``` python
import sys
from PyQt5.QtWidgets import QApplication, QWidget
from PyQt5.QtGui import QIcon

class MyApp(QWidget):

  def __init__(self):
      super().__init__()
      self.initUI()

  def initUI(self):
      self.setWindowTitle('Icon')
      self.setWindowIcon(QIcon('web.png'))
      self.setGeometry(300, 300, 300, 200)
      self.show()

if __name__ == '__main__':
  app = QApplication(sys.argv)
  ex = MyApp()
  sys.exit(app.exec_())
```

```lua
self.setWindowIcon(QIcon('web.png'))
```

- **setWindowIcon()** - 어플리케이션 아이콘을 설정

- QIcon 객체를 생성하고

- 보여질 이미지('web.png')를 입력 (이미지 파일을 다른 폴더에 따로 저장해 둔 경우 - 경로까지 함께 입력)

   

```lua
self.setGeometry(300, 300, 300, 200)
```

- **setGeometry()** - 창의 위치와 크기를 설정 == move() + resize()
- ( x위치, y위치, 너비, 높이 ) 





## 창 닫기

``` python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton
from PyQt5.QtCore import QCoreApplication

class MyApp(QWidget):
  def __init__(self):
      super().__init__()
      self.initUI()

  def initUI(self):
      btn = QPushButton('Quit', self)
      btn.move(50, 50)
      btn.resize(btn.sizeHint())
      btn.clicked.connect(QCoreApplication.instance().quit)

      self.setWindowTitle('Quit Button')
      self.setGeometry(300, 300, 300, 200)
      self.show()

if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())
```


```javascript
from PyQt5.QtCore import QCoreApplication
```

```ini
btn = QPushButton('Quit', self)
```

- 푸시버튼을 하나 만드는 데, 이 버튼 (btn)은 QPushButton 클래스의 인스턴스

- 생성자 (QPushButton()) 에는 ( 버튼에 표시될 텍스트, 버튼이 위치할 부모 위젯 ) 입력

   

```scss
btn.clicked.connect(QCoreApplication.instance().quit)
```

- PyQt5에서의 이벤트 처리는 **시그널과 슬롯(Signal & Slot)** 메커니즘으로 이루어짐 ( 추후 다룰 예정 )
- 버튼(btn)을 클릭하면 
  - 'clicked' 시그널이 만들어짐
  - instance() 메서드는 현재 인스턴스를 반환
  - 'clicked' 시그널은 어플리케이션을 종료하는 quit() 메서드에 연결됨
- 이렇게 발신자 (Sender)와 수신자 (Receiver), 두 객체 간에 커뮤니케이션이 이루어지는데,
- 이 예제에서 발신자는 푸시버튼 (btn), 수신자는 어플리케이션 객체 (app)



## 메뉴바

``` python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QAction, qApp
from PyQt5.QtGui import QIcon

class MyApp(QMainWindow):

    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        exitAction = QAction(QIcon('exit.png'), 'Exit', self)
        exitAction.setShortcut('Ctrl+Q')
        exitAction.setStatusTip('Exit application')
        exitAction.triggered.connect(qApp.quit)

        self.statusBar()

        menubar = self.menuBar()
        menubar.setNativeMenuBar(False)
        filemenu = menubar.addMenu('&File')
        filemenu.addAction(exitAction)

        self.setWindowTitle('Menubar')
        self.setGeometry(300, 300, 300, 200)
        self.show()

if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())
```

```lua
exitAction = QAction(QIcon('exit.png'), 'Exit', self)
exitAction.setShortcut('Ctrl+Q')
exitAction.setStatusTip('Exit application')
```

- 이 세 줄의 코드를 통해 아이콘 (exit.png)과 'Exit' 라벨을 갖는 하나의 동작 (action)을 만들고, 이 동작에 대해 단축키 (shortcut)를 정의합니다.

-  **setStatusTip()** - 메뉴에 마우스를 올렸을 때, 상태바에 나타날 상태팁을 설정

   

```scss
exitAction.triggered.connect(qApp.quit)
```

- 이 동작을 선택했을 때, 생성된 (triggered) 시그널이 QApplication 위젯의 quit() 메서드에 연결되고, 어플리케이션을 종료시킴

   

```python
menubar = self.menuBar()
menubar.setNativeMenuBar(False)
fileMenu = menubar.addMenu('&File')
fileMenu.addAction(exitAction)
```

- **menuBar()** -메뉴바를 생성
- 이어서 'File' 메뉴를 하나 만들고, 거기에 'exitAction' 동작을 추가
- '&File'의 **앰퍼샌드(ampersand, &)** - 간편하게 단축키를 설정하도록 해줌
  - 'F' 앞에 앰퍼샌드가 있으므로 'Alt+F'가 File 메뉴의 단축키가 되고, 만약 'i'의 앞에 앰퍼샌드를 넣으면 'Alt+I'가 단축키가 됨