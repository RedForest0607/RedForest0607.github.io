---
title: "[Dart]Dart에서 Getter와 Setter"
author: InGyuJang
date: 2023-03-08 10:20:00 +0900
categories: [Tech, Infrastructure]
tags: [TIL, Tech, Dart, Flutter]
pin: false
---

![](https://media.giphy.com/media/jUwpNzg9IcyrK/giphy.gif)  
  
  
# 📌 Dart에서 Getter와 Setter

여느때와 같이 `Peach-Tri`에서 스터디를 진행중이었다. 진행 도중에 `riverpod` 예시코드들에 getter setter 세팅이 전혀 없다는 점을 팀원이 이상하게 느꼈고, 그걸 기점으로 `Dart`라는 언어의 지향점이 어딘지에 대해서 의문을 품게 되었다. 너무나도 당연하게 `Java`스타일로 Getter Setter를 정해놓던 우리는 코드 작성 시간보다 디자인 패턴 파악에 긴 시간을 쏟은 결과, `Dart`의 도큐먼트와 몇몇 글들을 알게 되었고, 시작은 `Dart`지만 아마 이 내용은 전반적인 프로그래밍 언어의 흐름에 대해서 일 수도 있을 것 같다.

# 📎 Getter? Setter?
  
## ☕ In Java
`Java`에서 Getter와 Setter는 필수적이다. `Lombok`이라는 라이브러리를 활용해서라도, 이 불편한 accessor를 사용하게 한다. 이는 `OOP`의 원칙 중 하나인 `캡슐화`를 이뤄내기 위함이며, CLASS 내부적으로 private로 선언한 field들에 대해서 접근을 컨트롤 할 수 있는 하나의 입출구로 생각하면 이해할 수 있다. 학부생정도에서, 그리고 깊지 않은 프로그래밍 공부에서는 DTO나 Entity의 역할을 하는 클래스들이 줄줄이 getter와 setter를, 그저 `단순접근을 제어하기 위해`라는 목적하에 줄줄이 달려있는 것을 알 수 있다. 다만 이런 접근자들이 진짜로 필요한지에 대해서 재고해야한다.  
  
과연 그저 모든 field들에 getter와 setter를 붙이는 것 만으로 `캡슐화`라는 목표를 이룩할 수 있는 것일까?
  
## 🎯 In Dart
다트는 이런 단순접근자들을 사용하는것을 지양하라고 나와있다. 
>AVOID wrapping fields in getters and setters just to be "safe".  
  
  
단순한 안전을 위해서, getter와 setter를 통해서 필드들을 매핑하는것은 의미가 없다는 말이다
    
# 🤦 근데 이제와서 왜 우리는 사용하지 않는걸까?
```java
class Example {
  private String name;
  private int age;
  
  public void setName (String name) {
    this.name = name;
  }
  public String getName (String name) {
    return this.name;
  }
  public void set age (int age) {
    this.age = age;
  }
  public int getAge (int age) {
    return age;
  }
}
```
다음과 같은 자바 코드가 있다고 가정할 때, 과연 `캡슐화`되어 있는지 생각해보자, 클래스의 내부 field들은 private 상태이다. 하지만 정작 모든 field들이 public getter와 setter로 인해서 노출되어 있는 상태이다. 외부에서 이 class를 활용하려고 한다면, 내부적으로 어떤 구조를 하고 있는지, 어떤 형태를 띄고 있는지 알아야 하고, 이런 구조는 getter setter없이 그냥 `.`을 사용하는 것과 별반 다를 바 없는 구조를 갖게 되는 것이다. 이것은 언뜻 느끼기에는 `캡슐화`를 달성한 것 같아 보인다. 하지만 궁극적으로 `추상화`를 목표로 하는 `OOP`에서는 오히려 추상화를 해치고 내부 구조를 드러낸 것과 다를바 없는 코드가 되어 버린다. 단순 데이터 전달 목적이 아닌, 나름의 역할과 책임을 가진 클래스가 이러한 상태라면 캡슐화가 정상적으로 작동한다고 보기 어렵다.  
  
  
  
```java
...
public boolean isSame(Example example) {
  return (example.age == someAge) ? true : false;
}
...
```

  
접근제어자들은 API 즉 인터페이스들이고 인터페이스는 `내부로직`을 숨기고 객체에게 역할을 부여하며, 외부에서 다른 클래스들이 내부 값을 가져가 비교하거나 변환하는것이 아닌, 해당객체에게 값을 주면서 `질문`을 해야한다는 것이 핵심이다. 아래의 메소드는 Example내부에 존재하며, private한 값을 반환하지 않고도, 값을 비교할 수 있게 된다.

```java
class Example {
  ...
  
  public boolean isSame (int age) {
    return (age == this.age) ? true : false;
  }
  public boolean isSame (Stirng name) {
    return (name == this.name) ? true : false;
  }
  
  ...
}
```
위의 예시들은 극도로 단순하여 차라리 getter setter없이 field를 바로 가져오는게 더 실용적이겠지만, 더 복잡하고, 혹은 더 다양한 순간들에서 단순한 값 전달을 좀 더 `확실하게`하기 위해서 getter와 setter를 사용하는 것은 오히려 캡슐화를 하고 있다는 착각에 사로잡힐 수 있는 것이다.  
캡슐화는 정보은닉, 그리고 더 나아가서 `OOP`의 궁극적 목표인 추상화에 도달하기 위해, 그리고 각 클래스에 역할과 책임을 부여하기 위해서 각 객체들이 서로 커뮤니케이션을 위해서 구조를 알아야 하는 것이 아니라, 각 객체들이 서로에게 질문을 던지고 답변을 들으며 커뮤니케이션을 한다고 생각하면 편하다.

# 💁 그렇다면 적절한 Accessor의 사용은 어떤 때 일까?
Accessor들을 활용하기 가장 적합한 위치는 바로 `추가적인 연산`이나 `데이터에 변동`이 필요한 때이다. getter나 setter를 통해서 값이 바로 들어가는 것이 아니라, 그 중간에 값을 빼온다는지, 데이터에 따라서 따로 가공이 필요한 케이스의 경우에는 이런 getter setter를 통해서 중간다리 역할을 해줄 수 있다. 이런경우는 의심치 않고 getter setter를 활용하는 것이 도움이 될 것이다. 혹은 입력값에 따라서 Data내부에 있는 Type으로 변환하는 경우도 있을 수 있다. Data가 품고 있는 값을 `String`으로 입력된 이름으로 가져오고 싶다던지하는 경우는 getter setter를 활용할 여지가 충분히 존재한다.

---

## 📖 참고자료

- https://erik-engheim.medium.com/to-hell-with-setters-and-getters-7814e7b2f949

- https://dart-lang.github.io/linter/lints/unnecessary_getters_setters.html

- https://stackoverflow.com/questions/61720221/why-should-i-avoid-wrapping-fields-in-getters-and-setters

- https://stackoverflow.com/questions/1568091/why-use-getters-and-setters-accessors
