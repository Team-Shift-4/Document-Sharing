# by lazy

``` kotlin
class MainActivity : AppcompatActivity() {
	lateinit var binding : ActivityMainBinding
    
    override fun onCreate() {
        super.onCreate()
        binding = ActivityMainBinding.inflate(layoutInflator)
        setContentView(binding.root)
    }
}
```

# lateinit

``` kotlin
class MainActivity : AppcompatActivity() {
	val binding by lazy { ActivityMainBinding.inflate(layoutInflator) }
    
    override fun onCreate() {
        super.onCreate()
        setContentView(binding.root)
    }
}
```

- layoutInflator 는 늦게 만들어지기때문에 시점을 연기해야함
- 확실하게 만들어지는 부분이 super.onCreate() 임

- 두 개의 차이는 val 과 var의 차이 
  - 위는 가독성이 좋은 것



# null 허용 타입

``` kotlin
class MainActivity : AppcompatActivity() {
 	var _binding : ActivityMainBinding? = null
    val binding get() = _binding!!
    
    override fun onCreate() {
        super.onCreate()
        _binding = ActivityMainBinding.inflate(layoutInflator)
        setContentView(binding.root)
    }
}
```



# Companion object

``` kotlin
class A {
    var a = 0
    companion object {
        var b = 0
    }
}
```

- 객체를 여러개 생성해도 하나만 메모리에 잡힘
- 클래스 이름으로 불러서 씀 `A.b`
- 객체자체를 불러쓸 땐 이름 companion
- java static 역할과 동일하지만, 정확히는 static과는 다름
  - 변수도 들어갈 수 있고, 함수도 들어갈 수 있기 때문
  - static 객체, new 객체 생기는 공간이 다름



## Companion object 왜쓰냐⭐

``` kotlin
class LoginResponse(val code:Int, val msg:String) {    
    companion object {
        val LOGIN_OK = 0
        val LOGIN_NO_USER = 1
        val LOGIN_PASSWORD_ERR = 2
    }
}

val res = LoginResponse(LoginResponse.LOGIN_OK, "OK")
val res2 = LoginResponse(LoginResponse.LOGIN_OK, "OK")
```

- 여러 객체에서 같은 내용을 쓰니까 메모리를 여러번 잡아먹을 필요가 없음

- 객체 수만큼 필요하진 않지만 값은 필요할 때 사용

  - ex) 결과코드, 상태코드 등

  

### 대표적인 예제

``` kotlin
Toast.makeText(context, "text", Toast.LENGTH_SHORT).show()
```

