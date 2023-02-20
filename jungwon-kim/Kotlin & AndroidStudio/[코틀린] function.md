

# lambda expression 

파라미터를 받고, 한 개의 값으로 바뀜

```
{ a, b -> a + b }
```



## 📌 함수 파라미터로 함수를 받는 경우

- 파라미터, 반환 타입

``` kotlin
fun test(f:() -> Unit){
    println("test calls f()")
    f()
}

fun myFun(){
    println("function test")
}

fun main() {
    // 함수 파라미터로 함수를 받는 예제
    test(fun(){ println("function") })
    test { println("lambda") }
    test(::myFun) 
}
```

- 현재 파일에 있다는 표시 :: 
- 함수 넘겨줄 때 괄호 열고 닫으면 안 됨



### 다른 파라미터도 있을 때

``` kotlin
fun test2(value:Int, f:() -> Unit){
    println("test calls f()")
    f()
}

fun main() {
    test2(1, fun(){ println("function") })
    test2(1) { println("lambda") }
    test2(1, ::myFun) 
}
```

``` kotlin
fun test(str:String, f:(String)->Unit){
    f(str)
}

fun main() {
    test("A", fun(value:String){ println("function - $value")})
    test("A"){ println("lamdba - $it")}
}
```

- 함수 파라미터 안에 함수 파라미터는 변수 이름이 필요없음
  - `it`으로 사용하면 됨
  - 만약 람다식 안에 다른 파라미터 있어야한다면 이름 지어줘야 함!



### 예제

사칙연산 함수를 선언하고 배열에 담아 반복문 돌면서 실행

``` kotlin
fun add(n1: Int, n2: Int) = n1 + n2
fun sub(n1: Int, n2: Int) = n1 - n2
fun mul(n1: Int, n2: Int) = n1 * n2
fun div(n1: Int, n2: Int) = if (n2 == 0) 0 else n1 / n2

fun main() {
    val ops = arrayOf(::add, ::sub, ::mul, ::div, { n1, n2 -> n1 + n2 + 1 })
    ops.forEach { 
        println(it(10, 5)) // 15 \n 5 \n 50 \n 2 \n 16
    } 
} 
```



### varags

- 가변 개수의 파라미터 받을수 있음

  ``` kotlin
  // 가변 개수 파라미터 받아서 더하는 함수
  fun sum(vararg v: Int): Int {
      var sum = 0
      v.forEach { sum += it }
      return sum
  }
  
  // 배열을 받아서 더하는 함수랑 비교해보기
  fun sumArr(v:IntArray):Int {
      var sum = 0
      v.forEach { sum += it }
      return sum 
  }
  
  fun main(){
      println(sum()) // 0
      println(sum(1, 2, 3, 4, 5)) // 15
      print(sumArr(intArrayOf(1, 2, 3))) // 6
  }
  ```

  - 파라미터 갯수에 따라 결정해서 사용하면 됨

  

### spread

- 가변 파라미터 함수 사용



## Tail recursion

함수 호출이 제일 마지막 줄에 있으면 메모리를 계속 잡을 이유가 없음

- 재귀 호출 함수를 tail recursion 으로 작성했다면 `tailrec` 키워드 사용해 최적화 → 성능 높아짐

``` kotlin
tailrec fun test(n:Int){
    if(n==0) return
    println(n)
    test(n-1)
}

```



# 📌고차원 함수(higher order function)

## 함수형 언어?

함수가 First-class를 만족하면 !

- 함수를 파라미터로 받을 수 있음
- 함수의 실행 결과로 함수를 반환
- 함수를 변수나 자료구조(배열)에 저장가능
- 함수를 객체의 하나처럼 반환할 수 있으면 함수형 언어

👉 파라미터가 무엇이고 반환 타입이 뭔지가 중요
