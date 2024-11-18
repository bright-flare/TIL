# 아토믹 코틀린 학습중에 기록하고 싶은 것들.

- 세미콜론 생략해서 사용하는게 10분만에 적응될 정도로 편함
- Any Type은 java의 Object와 같은 역할을 한다. equals, hashcode 구현되어있음.
    - 같은 역할이지 Object는 아닌듯.
    - 최상위 객체인건 맞음.

- Unit 타입은 java의 void와 같다.
    - The type with only one value: the Unit object. This type corresponds to the void type in Java.

- void 생략 가능

- 타입을 콜론 이후에 선언해준다.

- kt 파일에 function을 마구마구 생성할 수 있음.
- val, var java final 키워드를 생략할 수 있어서 편하네.
- string template은 편하다 

```kotlin
val name = "test"
println("캬 ~ ${name}")
```

- raw string은 편하다 

```kotlin

  // raw string !
  val lines: String = """ 
    첫
    둘
    셋 줄 테스트 => ${words}
  """.trimIndent()

```

- 한줄짜리 함수를 만들 수 있따.. 내기준 충격
    - expression body라고 한다. 
    - 식본문
    - lambda expression 처럼 return 생략 가능함 !!
    - 식 본문만 반환 타입 추론 가능하다. 와우
```kotlin
fun multiplyByThree(number: Int): Int = number * 3

fun multiplyByFour(number: Int) = number * 3 // 식 본문만 반환 타입 추론 가능
```
- 함수 본문이 중괄호로 둘러싸인 경우 block body 블록 본문이라고 한다.