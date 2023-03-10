# Vi Editer

- 
    - 학습 사이트
        
        [웹 튜토리얼](https://openvim.com/)
        
        [게임 형식](https://vim-adventures.com/)
        
        [퀴즈 형식](http://vimgenius.com/)
        

---

Unix 환경에서 가장 많이 쓰이는 문서 편집기

한 화면을 편집하는 VIsual editor라는 뜻에서 유래

리눅스 배포나에 포함되는 VIM(Vi IMporved)

# 구조

## 명령 모드(command mode)

처음 vi를 시작하게 되면 들어가는 모드

방향키를 이용해 커서를 이동할 수 있음

입력 모드에서 `esc` 키를 눌러 명령 모드로 돌아갈 수 있음

명령어 사용이 가능

## 입력 모드(insert mode)

명령 모드에서 `i`나 `a` 명령을 통해 입력 모드로 넘어갈 수 있음

코드나 글을 작성할 수 있음

## 마지막 행 모드(last line mode)

명령 모드에서 `:`을 입력해 마지막 행 모드로 넘어갈 수 있음

# 명령어

## 명령 모드

`enter`를 치지 않아도 명령이 들어감

| 명령어 | 동작 |
| --- | --- |
| i | 현재 커서 위치에 삽입(입력 모드로 전환) |
| a | 현재 커서 바로 다음 위치에 삽입(입력 모드로 전환) |
| o | 현재 줄 다음 위치에 삽입(입력 모드로 전환) |
| (N)x | 커서 위치의 글자 N개 삭제(기입 없으면 1개) |
| dw | 커서 위치의 단어 삭제 |
| (N)dd | 커서 위치의 N개의 줄 잘라내기(기입 없으면 1개) |
| u | 이전 명령 취소 |
| (N)yy | 커서 위치의 N개의 줄을 버퍼로 복사(기입 없으면 1개) |
| p | 현재 커서가 있는 줄 바로 아래에 버퍼 내용 붙여넣기 |
| k | 커서가 한 줄 위로 올라감 |
| j | 커서가 한줄 아래로 내려감 |
| l | 커서가 한칸 우측으로 감 |
| h | 커서가 한칸 좌측으로 감 |
| 0 | 커서가 있는 줄의 맨 앞으로 감 |
| $ | 커서가 있는 줄의 맨 뒤로 감 |
| ( | 현재 문장의 처음 |
| ) | 현재 문장의 끝 |
| { | 현재 문단의 처음 |
| } | 현재 문단의 끝 |
| [N]- | N개의 줄만큼 위로 이동 |
| [N]+ | N개의 줄만큼 아래로 이동 |
| G | 파일의 끝으로 이동 |
| r | 한 문자 변경 |
| cc | 커서가 있는 줄의 내용 변경 |

## 마지막 행 모드

`enter`를 입력해야 명령이 들어감

| 명령어 | 동작 |
| --- | --- |
| w [file name] | 기입한 파일명으로 파일 저장(기입 없을시 현재 파일명으로 저장) |
| q | vi 종료 |
| q! | vi 강제 종료 |
| wq | 저장 후 종료 |
| wq! | 강제 저장 후 종료 |
| f [file name] | 기입한 파일명으로 파일명 변경 |
| [N] | N번 줄로 이동 |
| $ | 파일의 맨 끝 줄로 이동 |
| e! | 마지막 저장 이후 모든 편집 취소 |
| /[String] | 현재 커서 위치부터 앞쪽으로 기입한 문자열 검색 |
| ?[String] | 현재 커서 위치부터 뒤쪽으로 기입한 문자열 검색 |
| set nu | vi 라인 번호 출력 |
| set nonu | vi 라인 출력 취소 |