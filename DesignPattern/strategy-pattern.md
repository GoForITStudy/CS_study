# Strategy Pattern 전략 패턴

**개방 폐쇄 원칙(Open-Closed Principle)** 에 따라 변경이 발생하는 부분을 나머지 코드에 영향을 주지 않도록 분리하여 추상화, 캡슐화한다.

또한 **의존 역전 원칙(Dependency Inversion Principle)** 에 따라, 구현이 아닌 추상에 의존하도록 설계한다. 즉, 클래스 간의 의존, 결합도를 낮춘다.

이러한 디자인을 strategy pattern 이라고 부른다.
**스프링의 DI 의존성 주입** 이 이 전략 패턴을 완벽하게 활용한 예이다.

>행위(behavioral) 알고리즘을 캡슐화하여 동적으로 구현체를 교환하여 사용할 수 있도록 하며, 이를 사용하는 클라이언트와는 독립적으로 구현되므로, 결합도를 낮춘다.
즉, 특정 행동을 담당하는 인터페이스를 생성하여 관리할 수 있다.

# 전략 패턴

디자인 패턴의 꽃이라 불릴 만큼, 다양하게 자주 사용되는 패턴이다. 개방 폐쇄 원칙의 실현에도 가장 잘 들어맞는 패턴이라고 할 수 있다.

전략 패턴은 자신의 기능 맥락context에서 경우에 따라 **변경이 필요한 알고리즘** *(독립적 책임으로 분리가 가능한 기능을 말한다. 이를 **대체 가능한 전략**으로 본다.)* 을 **인터페이스를 통해 외부로 분리시킨 다음, 이를 구현한 구체적인 알고리즘 클래스를 요구에 맞게 바꿔서 사용**할 수 있게 하는 디자인 패턴이다.

전략 패턴에서 **컨텍스트(Dao 클래스)를 사용하는 클라이언트(Test 클래스 등)는 컨텍스트가 사용할 전략(ConnectionMaker 클래스)을 컨텍스트의 생성자 등을 통해 제공**해주는 게 일반적이다.

## SimUDuck 문제

### 문제상황
- 아래 예시의 Duck 클래스의 행위들, 즉 quack() 메소드와 fly() 메소드는 잦은 변경과 확장이 발생할 수 있다.
    - 이러한 메소드들을 모두 @Override 하는 형태로 관리하는 것은 어려운 일이다.
```java
public class Duck {
    void quack() {
        System.out.println("quack!quack!");
    }
    void swim() {}
    void display() {}
    void fly() {}
}

public class RubberDuck extends Duck {
    @Override
    void display() {
        System.out.println("노오란 러버덕");
    }
    @Override
    void quack() {
        System.out.println("Bip!Bip!");
    }
    @Override
    void fly() {
        System.out.println("날지 못하는 러버덕");
    }
}
```
### 행위의 추상화

- 잦은 변경이 발생하는 행위를 모아서 따로 클래스를 만들어 추상화, 캡슐화를 한다.
```java
public interface FlyBehavior {
    void fly();
}

public class FlyNoWay implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("fly no way");
    }
}

public class FlyWithWings implements FlyBehavior{

    @Override
    public void fly() {
        System.out.println("flying! 파닥파닥~");
    }
}

public class FlyRocketPowered implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("Flying Powered by Rocket");
    }
}
```
- Duck 클래스에서는 `행동 인터페이스 타입의 매개변수`를 두고, 각 하위클래스가 초기화될 때, 추상화된 인터페이스를 구현한 클래스를 할당하여 행동을 위임한다.
```java
public class Duck {
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;

    // behavior 클래스에 위임
    public void performFly() {
        flyBehavior.fly();
    }
    public void performQuack() {
        quackBehavior.quack();
    }
    public void swim() {
        System.out.println("every duck can swim, even decoy duck!");
    }
    public void display() {}

    public void setFlyBehavior(FlyBehavior flyBehavior) {
        this.flyBehavior = flyBehavior;
    }

    public void setQuackBehavior(QuackBehavior quackBehavior) {
        this.quackBehavior = quackBehavior;
    }
}

public class ModelDuck extends Duck {
    public ModelDuck() {
        flyBehavior = new FlyNoWay();
        quackBehavior = new Quack();
    }

    @Override
    public void display() {
        System.out.println("I'm a model duck.");
    }
}
```

### 동적으로 구현체 할당

- setter 메소드를 이용하여, 이러한 구현체를 동적으로 지정할거나 수정할 수도 있다.
- 혹은 해당하는 추상화 객체를 매개변수로 갖는 생성자를 활용할 수도 있다.
```java
public class MiniDuckSimulator {
    public static void main(String[] args) {
        Duck mallard = new MallardDuck();
        mallard.performFly();
        mallard.performQuack();

        Duck modelDuck = new ModelDuck();
        modelDuck.performFly();
        modelDuck.setFlyBehavior(new FlyRocketPowered()); // 동적으로 행동을 지정하는 위임 클래스 변경
        modelDuck.performFly();
    }
}
```