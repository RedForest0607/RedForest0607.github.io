---
title: JAVA의 Overloading, Overriding
author: InGyuJang
date: 2022-06-20 14:28:50 +0900
categories: [Tech, JAVA]
tags: [JAVA, TIL]
pin: false
---
오버로딩에는 메서드 오버로딩과 생성자 오버로딩이 있다.  
  
## 오버로딩  
---

같은 이름의 함수를 여러개 정의하고, 매개변수와 유형을 다르게 해서 다양한 유형의 호출에 응답할 수 있게 하는 것을 **오버로딩**이라고 한다.
  
```java
class OverloadingTest{
	void cat(){
		System.out.println("매개변수 없음");
	}
	void cat(String sound){
		System.out.println(sound);
	}
	void cat(int number){
		system.out.println("number is " + number);
	}
}

public cass OverTest{
	public static void main(String[] args){
		OverloadingTest testObject = new OverladingTest();

		testObject.cat();      //"매개변수 없음"

		testObject.cat("Mew"); //"Mew"
		
		testObject.cat(3);     //"number is 3"
	}
}
```

오버로딩은 다음과 같이 car()이라는 같은 이름의 메소드에 대해서 다른 파라미터를 받아서 실행되는 형식으로 작동한다.  
  
## 오버라이딩
---

클래스의 변수가 상속되는 것 처럼, 메소드도 상위에서 하위로 갈 수 있다. 또한, 하위클래스에서 메서드를 재정의 해서 사용할 수 있다. 이를 **오버라이딩이라고 한다.**

오버라이딩은 매서드의 이름이 같고, 매개변수도 같고, 반환형도 같을 경우, 상속받은 메서드를 덮어쓰는 방식으로 사용하겠다는 것을 뜻한다.

```java
class man{
	public String name;
	public int age;

	public void info(){
		System.out.println("남자의 이름은" + name", 나이는 " + age + "살입니다.");
	}
}

class job extends man(
	String job;

	public void info() {
		super.info();
		System.out.println("여자의 직업은 " + job + "입니다.");
	}
}

public class OverTest{
	public static void main(String[] args){
		Job job = new Job();

		job.name = "인규";
		job.age = 26;
		job.job = "프로그래머";

		job.info();     //남자의 이름은 인규, 나이는 26살입니다.
										//인규의 직업은 프로그래머입니다.
	}
}
```