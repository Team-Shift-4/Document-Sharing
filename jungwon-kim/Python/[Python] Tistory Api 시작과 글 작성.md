#TISTORY API



## [1. Open API 사용하기 위해 App등록 - Key 발급](https://www.tistory.com/guide/api/manage/register)

- 서비스명과 설명은 아무거나 상관 없음

- 서비스 URL : 본인의 블로그 주소 입력

- 서비스 형태 : 웹서비스

- 서비스 권한 : 읽기, 쓰기

- CallBack : 본인의 블로그 주소 입력

  

- 앱 관리 - 인증관리 - 설정 - 발급받은 App ID, Secret Key 가 필요함!



## 2. 인증 요청 및 Authentication code 발급

``` 
https://www.tistory.com/oauth/authorize?
  client_id={client-id}
  &redirect_uri={redirect-uri}
  &response_type=code
  &state={state-param}
```

- client_id: 클라이언트 정보의 Client ID 입니다.

- redirect_uri: 사용자가 인증 후에 리디렉션할 URI입니다. 클라이언트 정보의 Callback 경로로 등록하여야 하며 등록되지 않은 URI를 사용하는 경우 인증이 거부됩니다.

- response_type: 항상 `code`를 사용합니다.

- state: [사이트간 요청 위조](https://en.wikipedia.org/wiki/Cross-site_request_forgery) 공격을 보호하기 위한 임의의 고유한 문자열이며 리디렉션시 해당 값이 전달됩니다. (필수아님)

  

주소창에 위 url 입력 후 창이 뜨면 허가 버튼 클릭!

```
https://www.tistory.com/oauth/authorize?
  client_id=10fa6f8495c8132fd302919b1bd2a000
  &redirect_uri=http://wonny.kim/
  &response_type=code
```



## 3. 리디렉션 처리

허가 후 뜨는 url에서 code 기억해두기

```
https://wonny.kim/?code="코드기억"&state={state-param}
```

- 발급받은 코드는 1시간만 사용 가능



## 4. Access Token 발급

``` 
https://www.tistory.com/oauth/access_token?
  client_id={client-id}
  &client_secret={client-secret}
  &redirect_uri={redirect-uri}
  &code={code}
  &grant_type=authorization_code
```

- client_id: 클라이언트 정보의 Client ID 입니다.
- client_secret: 클라이언트 정보의 Secret Key 입니다. 이 정보는 티스토리와 Client만이 공유해야하며 절대 외부에 공개되면 안됩니다.
- redirect_uri: 인증요청시 사용한 리디렉션 URL로 요청검증을 위해 사용합니다.
- code: 리디렉션으로 전달받은 code를 그대로 사용합니다.
- grant_type: 항상 `authorization_code`를 사용합니다.

위 url 입력하면 화면은 에러라는 화면이 뜨는데, F12 개발자모드에 네트워크 창 가면 access_token 발급되었음!



### 👉파이썬 코드로 발급받는 법!

``` python
pip install requests
```

``` python
import requests

client_id = '발급받은 APP ID'
client_secret = '발급받은 Secret Key'
code = '발급받은 Authentication Code'
redirect_uri = '자신의 블로그 주소'
grant_type = 'authorization_code'

url = 'https://www.tistory.com/oauth/access_token?'
data = {
	'client_id': client_id,
	'client_secret': client_secret,
	'redirect_uri': redirect_uri,
	'code': code,
	'grant_type': grant_type
}
r = requests.get(url, data)
print(r.text)
```



-  ⭐ 코드당 한 번만 가능하니 400 에러 떴을 때 2번 인증 요청 다시 해야함⭐

이제 발급받은 access_token 을 이용해 글을 작성해보자!



## 📌카테고리 확인

```
GET https://www.tistory.com/apis/category/list?
  access_token={access-token}
  &output={output-type}
  &blogName={blog-name}
```

- blogName: 블로그를 구분하는 식별자로 사용되며 티스토리 주소 `xxx.tistory.com`에서 `xxx`를 나타냄



###  👉 파이썬으로 확인하기

``` python
import requests

access_token = '토큰 값'
blogName = '블로그 이름 .tistory 앞의 값'
output = 'json'

url = 'https://www.tistory.com/apis/category/list?'
data = {
		'access_token': access_token,
		'output': output,
		'blogName': blogName,
		}
r = requests.get(url, data)
print (r.text)
```

 



## 📌 글 작성 API

```
https://www.tistory.com/apis/post/write?
  access_token={access-token}
  &output={output-type}
  &blogName={blog-name}
  &title={title}
  &content={content}
  &visibility={visibility}
  &category={category-id}
  &published={published}
  &slogan={slogan}
  &tag={tag}
  &acceptComment={acceptComment}
  &password={password}
```

- **blogName**: 블로그를 구분하는 식별자로 사용되며 티스토리 주소 `xxx.tistory.com`에서 `xxx`를 나타냄 (필수)
- **title**: 글 제목 (필수)
- content: 글 내용
- visibility: 발행상태 (0: 비공개 - 기본값, 1: 보호, 3: 발행)
- category: 카테고리 아이디 (기본값: 0)
- published: 발행시간 (TIMESTAMP 이며 미래의 시간을 넣을 경우 예약. 기본값: 현재시간)
- slogan: 문자 주소
- tag: 태그 (',' 로 구분)
- acceptComment: 댓글 허용 (0, 1 - 기본값)
- password: 보호글 비밀번호



### 👉 파이썬으로 작성하기

``` python
import requests


client_id = '자신의 App ID'
client_secret = '자신의 Secret Key'
access_token = '발급 받은 access token'
code = '발급 받은 가장 최신의 Authentication Code'
redirect_uri = '자신의 블로그 주소'
blogName = '블로그 이름 (https://XXX.tistory.com 에서 XXX의 값)'
tag = '테스트, test'					#등록할 태그값, 쉼표로 구분
output = 'json'						#고정값
grant_type = 'authorization_code'   #고정값 
visibility = 0  #0은 비공개, 3은 공개 발행
category =  1039213


def getToken():
    url = 'https://www.tistory.com/oauth/access_token?'
    data = {
            'client_id': client_id,
            'client_secret': client_secret,
            'redirect_uri': redirect_uri,
            'code': code,
            'grant_type': grant_type
            }
    r = requests.get(url, data)
    print (r.text)


def getCategory():
    url = 'https://www.tistory.com/apis/category/list?'
    data = {
            'access_token': access_token,
            'output': output,
            'blogName': blogName,
            }
    r = requests.get(url, data)
    print (r.text)


def postWrite(content):
        title = '자동 포스팅 테스트'
        url = 'https://www.tistory.com/apis/post/write?'
        data = {
                 'access_token': access_token,
                 'output': output,
                 'blogName': blogName,
                 'title': title,
                 'content': content,
                 'visibility': visibility,
                 'category': category,
                 'tag': tag,
                 }
        r = requests.post(url, data=data)
        print ('자동 포스팅 성공')
        return r.text


if __name__ == '__main__':
    # getToken()    #최초 토큰 발급시에만 수행
    # getCategory()  #최초 카테고리 확인시에만 수행
	postWrite('테스트 등록 글입니다.')
```



[파이썬 코드 참고 블로그](https://liwonfather.tistory.com/58)

[공식문서가기](https://tistory.github.io/document-tistory-apis/)

