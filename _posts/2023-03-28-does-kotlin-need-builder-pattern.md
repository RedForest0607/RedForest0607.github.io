---
title: "[Kotlin] 코틀린은 빌더 패턴을 필요로 할까?"
author: InGyuJang
date: 2022-03-28 13:50:20 +0900
categories: [ProgrammingLanguage, Kotlin]
tags: [Kotlin, ProgrammingLanguage, JVM]
pin: false
---
![](https://media.giphy.com/media/xT5LMJT72fM6WKP0is/giphy.gif)

#  엥 빌더 패턴?
디자인 패턴을 공부해 봤다면, `빌더`패턴에 대해서 알고 있을 것이다. 그래도 홍옥 시나 알쏭달쏭할만한 분들을 위해서 간략하게 설명을 하자면..

```
클래스 {
  빌더 {
    클래스 리턴하는 메서드 build()
  }
}
```
다음과 같은 구조로 클래스 내부의 클래스 프로퍼티들을 받아 클래스를 리턴하는 빌더를 생성하여서, 그 빌더를 통해서 클래스를 생성하는 방법이다.  
클래스 내부에 필수적인 프로퍼티와 Nullable한 프로퍼티들을 구분하여 사용할 수 있으며, 만약 새로운 Nullable한 프로퍼티가 생기더라도, 중간의 빌더 패턴을 한번 통해주면, 디폴트 값을 넣어주면서 모든 코드를 수정하지 않아도 되는 장점이 있다.

```java
public class Student {
  private final String name;
  private final int studentNumber;
  private final String hobby;
  private final String address;
  
  pubic Student(String name, Int studentNumber) {
    return this(name, studentNumber, null, null);
  }
  
  public Student(String name, Int studentNumber, String hobby) {
    return this(name, studentNumber, hobby, null);
  }
  public Student(String name, Int studentNumber, String hobby, String address) {
    return this(name, studentNumber, hobby, address);
  }
}
```
단순하게 생각한다면, 자바는 오버로딩을 통해서, 다양한 생성자 입력에 대해서 각각의 생성자메소드를 통해 생성해줘야 할것이다.

#### Effective Style Builder Pattern

```java
public class Student {
  private final String name;
  private final int studentNumber;
  private final String hobby;
  private final String address;

  private Student(Builder builder) {
    this.name = builder.name;
    this.studentNumber = builder.studentNumber;
    this.hobby = builder.hobby;
    this.address = builder.address;
  }

  public static class Builder {
    private final String name;
    private final int studentNumber;
    private String hobby = null;
    private String address = null;

    public Builder(String name, int studentNumber) {
      this.name = name;
      this.studentNumber = studentNumber;
    }

    public Builder hobby(String hobby) {
      this.hobby = hobby;
      return this;
    }

    public Builder address(String address) {
      this.address = address;
      return this;
    }

    public Student build() {
      return new Student(this);
    }
  }
}


.
.
.
Student.Builder builder = new Student.Builder("InGyu", 2023, "guiter", "Incheon");
Student student = builder.build();

or

Student student = new Student.Builder("InGyu", 2023)
                              .hobby("guitar")
                              .address("Incheon")
                              .build();
.
.
```
빌더패턴은 다음과 같이 내부 스태틱클래스를 통해서, 입력된 값에 따라 Builder인스턴스를 생성하여 Builder인스턴스가 클래스를 생성해 리턴해주는 디자인 패턴이다. 객체를 한번에 생성하며, 함수의 입력값이 맞는지, 필수적인 변수와 아닌 변수도 나눌 수 있게 된다.

## 🤦 To Kotlin.
그렇다면 코틀린은 어떨까? 먼저 코틀린으로 단순하게 마이그레이션 해보자

```kotlin
class Student private constructor(builder: Builder) {
    private val name: String
    private val studentNumber: Int
    private val hobby: String?
    private val address: String?

    init {
        name = builder.name
        studentNumber = builder.studentNumber
        hobby = builder.hobby
        address = builder.address
    }

    class Builder( val name: String, val studentNumber: Int, hobby: String?, address: String?) {
        var hobby: String? = null
        var address: String? = null
        fun hobby(hobby: String?): Builder {
            this.hobby = hobby
            return this
        }

        fun address(address: String?): Builder {
            this.address = address
            return this
        }

        fun build(): Student {
            return Student(this)
        }
    }
}
```
`intelliJ`에 그대로 복붙해서, 약간의 오류를 고쳐주고 나면 다음과 같은 코드가 자동으로 생성된다. Student의 생성자 파라미터로 Builder를 넣어주고, 이 Student는 Builder의 값들을 받아서 초기화 된다. Student의 내부 클래스 Builder는 파라미터들을 받아서 각각의 값들로 넣어주고, build()를 통해서 Student를 리턴하게 된다.

사실 `Builder`클래스는 결국 데이터를 핸들링 하는 것에 의미가 있기 때문에, 코틀린 Data Class를 사용해서 다음과 같이도 리팩토링이 가능하다.
```kotlin
class Student private constructor(
    val name: String,
    val studentNumber: Int,
    val hobby: String?,
    val address: String?
) {
    data class Builder(
        val name: String,
        val studentNumber: Int,
        var hobby: String? = null,
        var address: String? = null
    ) {
        fun address(address: String) = apply { this.address = address }
        fun hobby(hobby: String) = apply { this.address = address }
        fun build() = Student(name, studentNumber, hobby, address)
    }
}
```
코틀린 스타일이 가미된, 방식의 코드다. Builder가 Data Class로 존재하며, 파라미터들을 받아 초기화 되고, build()는 Student를 리턴한다.  
데이터 클래스를 통해서 마치 `Class의 DTO`처럼 데이터를 그대로 가져와 클래스를 생성해주는 이런 방식으로 빌더 패턴을 만들 수 있다.  


## 📎 conclusion
위의 예시만 봐도 알겠지만, 자바보다는 훨씬 더 간결한 코드로 보여줄 수 있으며, 그런 간결해진 문장을 통해서 자바의 코드보다 훨씬 더 이해가 쉽고 명확하다는 것을 알 수 있다. 또한, 코틀린의 라이브러리 함수들, `apply`와 같은 예시에 `DSL`에 대한 이해도 까지 더해진다면 정말 간결하고 명확한 코드들을 직접 짤 수 있게 된다.  

```kotlin
val student = Student.Builder("InGyu", 2023)
    .address("Incheon")
    .hobby("guitar")
    .build()
```