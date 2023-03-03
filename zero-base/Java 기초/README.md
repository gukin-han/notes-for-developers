# Java 기초

## 변수와 자료형

## 여러가지 연산자

## 조건문

## 반복문

## 다차원 배열

## 클래스와 객체

- 클래스와 객체의 관계를 이해
- 기본 사용 방법과 생성자 및 this의 사용

### 클래스 (class)

- 객체를 정의하는 설계도
    - 예) 자동차 설계도
        - (특성) 디자인, 무게, 가격
        - (기능) horn, break
    - 실체화 → car1, car2, …
- 쿠키 커터 → 쿠키1, 쿠키2, …, 쿠키n
- 요리법 → 요리1, 요리2, …., 요리n

> 🧷 객체: 사물, 실체
> 

## 객체, 인스턴스

- 객체 (Object)
    - 실체
- 인스턴스 (Instance)
    - 클래스와 객체의 관계
    - 클래스로부터 객체를 선언 (인스턴스 화)
    - 📌 **어떤 객체는 어떤 클래스의 인스턴스**

## 클래스 사용

- 클래스: 객체를 만들기 위한 설계도
    - 객체 변수, 메소드로 이루어짐

```java
	public class 클래스명 {
    // 객체 변수
    // 메소드 리턴타입 메소드명 (파라미터) {}
    // + 접근제어자
    // + static
}

클래스명 객체명 = new 클래스명();
```

## `this`, `this()`

- `this`
    - 객체 자신을 의미
- `this()`
    - 생성자

## 생성자

- 객체가 생성될 때 자동으로 호출됨
    - `new 클래스명();`
- 생성자 규칙
    - 클래스명과 이름 맞추기
    - 리턴 타입 없음
    - 생성자가 여러개 올 수 있다 (= 오버로딩)

```java
public class 클래스명 {
    클래스명() { // 생성자
}
```

## 실습

**📂 클래스 예제 - 전체 코드**

```java
class Car {
    String name;
    String type;

    Car() {
    }

    Car(String name, String type) {
        this.name = name;
        this.type = type;
        System.out.println("Constructor activate");
    }

    public void printCarInfo() {
        System.out.println("== Car Info ==");
        System.out.println("name = " + name);
        System.out.println("type = " + type);
    }

    public void move() {
        System.out.println("move" + this.name);
    }

    public void brake() {
        System.out.println("break active of" + this.name);
    }
}

public class Main {

    public static void main(String[] args) {
        Car myCar1 = new Car();
        myCar1.name = "MQ4";
        myCar1.type = "SUV";
        myCar1.printCarInfo();
        myCar1.move();
        myCar1.brake();

        System.out.println("");

        Car myCar2 = new Car("GRANDEUR", "Sedan");
        myCar2.printCarInfo();
        myCar2.move();
        myCar2.brake();
    }
}
```

**📂 클래스 예제 - 실행 결과**

```java
== Car Info ==
name = MQ4
type = SUV
moveMQ4
break active ofMQ4

Constructor activate
== Car Info ==
name = GRANDEUR
type = Sedan
moveGRANDEUR
break active ofGRANDEUR
```

## 상속

## 다형성

## 추상 클래스

## 인터페이스

## 내부 클래스

## 입출력

## 예외 처리

## 컬렉션 프레임워크

## 람다식

## 스트림

## 연습 문제