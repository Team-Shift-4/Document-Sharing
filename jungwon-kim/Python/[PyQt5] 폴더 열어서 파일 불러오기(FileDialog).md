pyqt를 이용해 파일을 읽어오기 전에, 파이썬 문법으로 파일을 읽어오는 방법을 알아보자



## 👉 파일 읽기

``` python
f = open("새파일.txt", 'w')
f.close()
```

| 파일열기 모드 | 설 명                                                      |
| ------------- | ---------------------------------------------------------- |
| r             | 읽기모드 - 파일을 읽기만 할 때 사용                        |
| w             | 쓰기모드 - 파일에 내용을 쓸 때 사용                        |
| a             | 추가모드 - 파일의 마지막에 새로운 내용을 추가 시킬 때 사용 |



'test.txt' 라는 파일에 적힌 내용을 경로로 가져와서 쓰고싶을 때

``` python
# 경로가 적힌 파일 불러오기
path_file = open("test.txt", 'r')
path_dir = path_file.readline()
path_file.close()
print(path_dir)
```

- python에서 경로

  - 슬래시나 역슬래시 2개 사용

  > 파이썬 코드에서 파일 경로를 표시할 때 `"C:/doit/새파일.txt"` 처럼 슬래시(`/`)를 사용할 수 있다. 만약 역슬래시(`\`)를 사용한다면 `"C:\\doit\\새파일.txt"` 처럼 역슬래시를 2개 사용하거나 `r"C:\doit\새파일.txt"`와 같이 문자열 앞에 `r` 문자(Raw String)를 덧붙여 사용해야 한다. 왜냐하면 `"C:\note\test.txt"`처럼 파일 경로에 `\n`과 같은 이스케이프 문자가 있을 경우 줄바꿈 문자로 해석되어 의도했던 파일 경로와 달라지기 때문이다.

  

``` python
# 경로안에 있는 파일 리스트 가져오기
file_list = os.listdir(path_dir)

# 파일 하나 선택해서 경로 저장
test_file = path_dir+"/"+file_list[7]
```

``` python
# 그 중 하나를 골라 내용 출력
test = open(test_file, 'r', encoding='UTF-8')
line = test.readlines()

for i in range(len(line)):
    print(line[i], end='')
```



파일을 읽으려고 할 때 아래와 같은 오류가 발생한다면 옵션에 `encoding='UTF-8'` 추가할 것

``` 
'cp949' codec can't decode byte 0x80 in position 7: illegal multibyte sequence
```

``` python
open(file,encoding='UTF-8')

open(file,'r',encoding='UTF-8')
```



[참고]([점프투파이썬](https://wikidocs.net/26))



pyqt를 이용한 파일 불러오기! 

##  👉 PyQt5 - QFileDialog

``` python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QTextEdit, QAction, QFileDialog
from PyQt5.QtGui import QIcon


class MyApp(QMainWindow):

    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.textEdit = QTextEdit()
        self.setCentralWidget(self.textEdit)
        self.statusBar()
        openFile = QAction(QIcon('open.png'), '파일 열기', self)
        openFile.triggered.connect(self.showDialog)

        menubar = self.menuBar()
        menubar.setNativeMenuBar(False)
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(openFile)

        self.setWindowTitle('File Dialog')
        self.setGeometry(300, 300, 300, 200)
        self.show()

    def showDialog(self):
        fname = QFileDialog.getOpenFileName(self, 'Open file', './')

        if fname[0]:
            f = open(fname[0], 'r', encoding="utf-8")

            with f:
                data = f.read()
                self.textEdit.setText(data)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())
```

`fname = QFileDialog.getOpenFileName(self, 'Open file', './')`

- QFileDialog를 띄우고, getOpenFileName() 메서드를 사용해서 파일을 선택함

``` python
if fname[0]:
    f = open(fname[0], 'r', encoding="utf-8")

    with f:
        data = f.read()
        self.textEdit.setText(data)
```

- 선택한 파일을 읽어서, setText() 메서드를 통해 텍스트 편집 위젯에 불러오기
- encoding 방식이 맞지 않아 열리지 않을 수 있으니 `encoding="utf-8"` 추가하기



[공식문서](https://doc.qt.io/qt-5/qfiledialog.html)