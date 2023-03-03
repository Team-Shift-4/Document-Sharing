

## 클래스

- ES2015+ 에 Class 도입

``` javascript
function Info(count) { // 빈 클래스인데 
    this.count = count; // count 라는 프로퍼티 추가해서 return
}

const obj = new Info(1); // obj 안에는 count 라는 프로퍼티가 있는 상태로 만들어짐
```

- 함수 이름이지만 대문자로 만들면 객체를 만들겠다는 의미 ⇒ 생성자 역할
- 모든 객체에 프로퍼티 추가하고싶다면 ⇒ 프로토타입(부모에 있는) 추가



``` javascript
// 기존 프로토타입 이용해서 상속하는 게 불편해서 만든 Class
class Human {
    constructor(type='human'){
        this.type = type
    }
    static isHuman(human) {
        return human instanceof Human;
    }
    breathe() {
        console.log('haha');
    }
}

class Zero extends Human {
    constructor(type, firstName, lastName) {
        super(type);
        this.firstName = firstName;
        this.lastName = lastName;
    }

    sayName() {
        super.breathe();
        console.log(`${this.firstName} ${this.lastName}`);
    }
}

const newZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(newZero);
```

