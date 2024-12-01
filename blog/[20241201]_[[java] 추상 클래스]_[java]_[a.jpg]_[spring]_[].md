# [java] 추상 클래스

## 서론

내가 다니는 회사에서는  
사용자 로그인 기능을 개발할 때, 로그인에 필요한 메소드들을 **추상 클래스**로 정의하고  
고객사들이 필요한 로직을 직접 구현할 수 있도록 어댑터 패턴을 적용했다.

## 선요약
추상 클래스는 프로그래밍에서 '설계도' 역할을 하며, 특히 대규모 프로젝트에서 일관성 있는 설계를 보장하고 필수 메소드 구현을 강제하는 데 유용하다.

추상 클래스의 네 가지 주요 특징:

- **인스턴스화 불가:** new 키워드로 직접 인스턴스를 생성할 수 없으며, 상속받은 구체 클래스를 통해서만 인스턴스화가 가능
- **추상 메소드 포함:** 선언만 있고 구현이 없는 추상 메소드를 포함할 수 있으며, 이는 하위 클래스에서 반드시 구현해야 함
- **일반 메소드 포함:** 구현이 있는 일반 메소드도 포함할 수 있어 하위 클래스에서 재사용 가능
- **상속을 통한 확장:** 다른 클래스가 상속받아 추상 메소드를 구현하고 확장할 수 있음


## 추상 클래스

추상 클래스는 주로 여러 클래스에서 공통적으로 사용될 수 있는 기능을 정의하고, 세부 사항은 하위 클래스에서 구현하도록 할 때 사용하는 클래스이다.

추상 클래스는 프로그래밍에서 일종의 '설계도' 같은 것이다.
여러 클래스들이 공통으로 가져야 할 **기본 틀**을 만들어두고, 
각각의 클래스들이 자신만의 방식으로 **세부 내용을 채워 넣을 수 있게** 해주는 특별한 클래스이다.

### 추상 클래스 예시

```java
abstract class Animal {
    // 추상 메서드 - 각 동물마다 다르게 구현해야 함
    abstract void makeSound();
    
    // 일반 메서드 - 모든 동물이 공통으로 사용
    void eat() {
        System.out.println("음식을 먹습니다.");
    }
}

// 구체 클래스
class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("멍멍!");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("야옹!");
    }
}
```

Animal이라는 기본 틀(추상 클래스)을 만들고,  
Dog, Cat 클래스가 자신만의 방식으로 세부 내용을 구현한다.  

### 추상 메소드를 사용하는 이유

추상 메소드를 사용하는 가장 큰 이유는 **일관성 있는 설계**를 보장하기 위해서이다.  
예를 들어, 여러 개발자가 함께 일하는 큰 프로젝트에서 특정 기능을 구현할 때:

- 모든 개발자가 반드시 구현해야 하는 메소드를 강제할 수 있다
- 실수로 필수 메소드를 빼먹는 것을 방지할 수 있다 (컴파일 에러 발생)
- 프로그램의 구조를 더 명확하게 만들 수 있다

실제 현업에서는 이런 특징이 매우 중요하다. 특히 여러 팀이 협업하는 대규모 프로젝트에서는:

- 각 팀이 자신들의 요구사항에 맞게 구현할 수 있으면서도
- 전체 시스템의 일관성과 안정성을 유지할 수 있다

이는 마치 건축에서 설계도면을 제공하는 것과 비슷하다.  
기본적인 구조는 정해져 있지만, 세부적인 내용은 각자의 필요에 따라 채워넣을 수 있는 것이다.

## 특징1 - 인스턴스화 불가

추상 클래스는 다른 클래스들이 상속받아 사용할 수 있는 기본 설계도 역할을 한다.
그래서 추상 클래스는 직접 인스턴스화 할 수 없다.   

인스턴스화 라는 것은 다른말로,   
**new 키워드로 인스턴스를 생성하는 것이 불가능**하다는 것이다.

### 예시:

```java
abstract class Vehicle {
    abstract void start();
}

public class Main {
    public static void main(String[] args) {
        // 컴파일 에러 발생!
        // Vehicle car = new Vehicle();  // 추상 클래스는 직접 인스턴스화 할 수 없음
        
        // 대신 추상 클래스를 상속받은 구체 클래스를 통해 인스턴스화 해야 함
        class Car extends Vehicle {
            void start() {
                System.out.println("자동차가 시동을 걸었습니다.");
            }
        }
        
        Vehicle car = new Car();  // 정상 동작
        car.start();
    }
}
```

Vehicle 추상 클래스는 직접 인스턴스화할 수 없다.  
그니까 new 키워드로 인스턴스를 생성하려고 하면 컴파일 에러가 난다.  
대신 이 추상 클래스를 상속받은 구체 클래스를 통해서만 인스턴스를 생성할 수 있다.


## 특징2 - 추상 메소드 포함

추상 클래스는 하나 이상의 추상 메소드를 포함할 수 있다.
(추상 메소드는 메소드의 선언만 있고 구현은 없는 메소드이다.)

+ 추상 클래스 안에 꼭 추상 메소드가 있을 필요는 없다.

그리고 이 추상 메소드는 하위 클래스에서 반드시 구현해야 한다.

### 예시:

```java
// 추상 클래스 정의
abstract class Shape {
    // 추상 메소드 선언 (구현 없음)
    abstract double calculateArea();
    abstract double calculatePerimeter();
}

// 하위 클래스에서 추상 메소드 구현
class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    // 추상 메소드 구현
    @Override
    double calculateArea() {
        return width * height;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * (width + height);
    }
}

// 다른 하위 클래스에서의 구현
class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    // 추상 메소드 구현
    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
}
```

Shape라는 추상 클래스에 
calculateArea()와 calculatePerimeter()라는 추상 메소드가 선언되어 있다.  
Rectangle과 Circle 클래스에서 각각 자신만의 방식으로 이 메소드들을 구현한다.


## 특징3 - 일반 메소드 포함 가능
추상 클래스는 일반 메소드(구현이 있는 메소드)도 포함 가능하다.  
공통적으로 사용될 수 있는 메소드를 정의하여 하위 클래스에서 재사용할 수 있다.(상속)


### 예시:

```java
abstract class Animal {
    // 추상 메서드 - 각 동물마다 다르게 구현해야 함
    abstract void makeSound();
    
    // 일반 메서드 - 모든 동물이 공통으로 사용
    void eat() {
        System.out.println("음식을 먹습니다.");
    }
}

// 구체 클래스
class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("멍멍!");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("야옹!");
    }
}
```

Animal 클래스에 있는 eat() 메소드는 Dog 와 Cat 이 사용할 수 있다.

## 특징4 - 상속을 통한 확장

추상 클래스는 다른 클래스가 상속받아 구체화할 수 있도록 설계 되어있다.

하위 클래스는 추상 클래스의 추상 메소드를 구현해야 하며, 필요에 따라 추상 클래스의 일반 메소드를 재정의(overriding)할 수 있다.

### 예시:

```java
// 추상 클래스 정의
abstract class Shape {
    // 추상 메소드 선언 (구현 없음)
    abstract double calculateArea();
    abstract double calculatePerimeter();
}

// 하위 클래스에서 추상 메소드 구현
class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    // 추상 메소드 구현
    @Override
    double calculateArea() {
        return width * height;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * (width + height);
    }
}

// 다른 하위 클래스에서의 구현
class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    // 추상 메소드 구현
    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
}
```

Shape 추상 클래스를 Rectangle , Circle 클래스가 상속 받았다.

Rectangle , Circle 클래스는 calculateArea, calculatePerimeter 메소드를 확장해서 구현할 수 있다.

