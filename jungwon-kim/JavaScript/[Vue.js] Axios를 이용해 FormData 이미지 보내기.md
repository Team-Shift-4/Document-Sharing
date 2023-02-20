현재 vue.js 와 springboot 를 통신 중인데,  이미지 업로드를 시도하고 있다.

form으로 전체를 감싸는 것 말고, 폼 데이터로 보내는 법을 기록해보겠다!



## vue.js

``` html
 <p>사진: <input type="file" @change="setPhoto" accept="image/*" id="photo"></p>
```

@change 를 활용해 input을 했을 때 setPhoto라는 함수를 실행하게 했다.

accept 로 파일은 image만 가능하게 한다.





``` javascript
 methods: {
        setPhoto(){
            let frm = new FormData();
            let photoFile = document.getElementById("photo");
            frm.append("photo", photoFile.files[0]);
            
            axios.post("/api/photo", frm, {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            })
            .then(()=>{
                console.log("전송 성공");
            })
            .catch((e)=> {
                console.log(e);
            })
        },
 }
```

`setPhoto` 에서는 `frm` 이라는 새로운 `FormData`를 만든다.

`frm` 은 Key, Value 쌍이므로 `frm.append("photo", photoFile.files[0]);` 하면 photo 라는 키로  값이 전송된다.



``` javascript
 headers: {
    'Content-Type': 'multipart/form-data'
}
```

파일 업로드를 axios로 처리하려면 헤더설정이 필요하므로 Content-Type을 필수로 명시해줘야한다!



***



### + SpringBoot 에서 받는 법

데이터를 받는 지 테스트 해보려고 만든 코드

``` java
  @PostMapping(value = "/api/photo", consumes = {MediaType.MULTIPART_FORM_DATA_VALUE})
    public void addPhoto(@RequestParam("photo") MultipartFile img){
        System.out.println(img.getOriginalFilename());
    }
```

`consumes = {MediaType.MULTIPART_FORM_DATA_VALUE}`라고 수신받을 데이타 포맷을 설정해줘야 Content-Type을 multipart/form-data라고 설정한 걸 받을 수 있음!