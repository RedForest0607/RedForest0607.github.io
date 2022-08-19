---
title: '[JAVA] JAVA의 Thread'
author: InGyuJang
date: 2022-07-08 19:48:20 +0900
categories: [Tech, JAVA]
tags: [JAVA, TIL]
pin: false
---
# 자바의 Thread  
![image](https://media.giphy.com/media/Kc1tJDZ3Q4d2pfalIG/giphy.gif)
  
### Thread

프로세스는 프로그램의 실행 단위로, 프로세스를 다중으로 실행하게. 되면 그 크기가 크고, 메모리 공유에 있어서 단점이 존재하기 때문에, Thread를 통해서 프로그램을 실행하는 것이 프로세스보다 자원을 덜 소모하면서도 효율적으로 멀티 태스킹이 가능하게 된다.

### Runnable vs Thread

자바에서 Thread는 Thread 클래스를 상속 받는 것과, Runnable 인터페이스를 implements하는 것 두가지로 나눌 수 있는데, 각각의 장단점이 존재한다.

runnable 객체는 스스로 Thread가 아니기 때문에, Thread객체를 생성하고, 그 생성자에서 다시 Interface를 통해 Runnable 객체를 생성해줘야한다.

### Thread 클래스 확장법

```dart
public class MyThread extends Thread{
	@Override
	public void run(){
	}
}
```

### Runnable implements 방법

```dart
public class MyRunnable implements Runnable{
	@Override
	public void run() {
	}
}

```

### 구현부

```dart
public class ThreadExam {
	public static void main(String[] args) {
		Thread thread1 = new MyThread();
	
		Thread thread2 = new Thread(new MyRunnable());
		Thread thread3 = new Thread(()->{원하는 구현});
	}
}
```

thread3과 같이 람다식을 통해서 간결하게 구현할 수 있다.

### 동기화 메소드

```dart
public synchronized void PrintLine() {
	for (int i = 0; i < 10 ; i ++) {
		synchronized(this){
			System.out.println("라인을 프린트합니다.");
		}
	}
}
```

`synchronized` 키워드를 통해서 객체를 공유하는 다른 쓰레드의 메소드과 동시에 호출되는 것이 아니라, 해당 부분이 실행되는 동안은 다른 메소드들이 호출되지 않고 해당하는 메소드가 먼저 객체를 사용하고 다른 메소드는 기다리도록 모니터링 락(Monitoring Lock)을 획득하여 실행된다.

### join()

join메소드는 쓰레드를 실행하고, 실행된 쓰레드가 종료될 때 까지 다른 쓰레드는 기다린다.