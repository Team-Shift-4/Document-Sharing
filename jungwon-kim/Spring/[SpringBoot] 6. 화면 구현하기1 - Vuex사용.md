[toc]



화면 구현하면서 서버 통신을 위해 Vuex, Axios를 사용하였으며

Vuex, Axios에 대한 자세한 내용은 다른 글에서 다루도록 하겠습니다.

# 📌 Vuex

- Vue.js 애플리케이션을 위한 **상태 관리 패턴 + 라이브러리**

- 상태가 예측 가능한 방식으로만 변경될 수 있도록 하는 규칙을 사용하여

  응용 프로그램의 모든 구성 요소에 대한 중앙 집중식 저장소 역할

# 📌 Axios

- 뷰에서 권고하는 HTTP 통신 라이브러리

- **불러오기** : axios.get(url[, config])
- **입력하기** : axios.post(url[, data[, config]])
- **수정하기** : axios.patch(url[, data[, config]])
- **삭제하기** : axios.delete(url[, config])

commit - post시 여러개 넘길 수 없음
