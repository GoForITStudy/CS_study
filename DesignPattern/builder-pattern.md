## 빌더 패턴이란?

인스턴스 **생성**과 관련한 패턴 

인스턴스를 만들 때 다양한 구성으로 만들어질 수 있는데, 다양한 구성으로 만들어진 인스턴스를 동일한 프로세스를 통해서 만들 수 있게 해주는 패턴 

## 빌더 패턴을 사용하는 이유…

변수 중 일부만 가지고 객체를 생성하고 싶을 때 사용

- **점층적 생성자 패턴**
    
    NutritionFacts -영양정보를 나타내는 클래스
    

```java
package item2.telescopingconstructor;

// 점층적 생성자 패턴 - 확장하기 어렵다! (14~15쪽)
public class NutritionFacts {
  private final int servingSize;  // (mL, 1회 제공량)     필수
  private final int servings;     // (회, 총 n회 제공량)  필수
  private final int calories;     // (1회 제공량당)       선택
  private final int fat;          // (g/1회 제공량)       선택
  private final int sodium;       // (mg/1회 제공량)      선택
  private final int carbohydrate; // (g/1회 제공량)       선택

	public NutritionFacts(int servingSize, int servings) {
		this(servingSize, servings, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories) {
		this(servingSize, servings, calories, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories, int fat) {
		this(servingSize, servings, calories, fat, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
		this(servingSize, servings, calories, fat, sodium, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
		this.servingSize  = servingSize;
		this.servings     = servings;
		this.calories     = calories;
		this.fat          = fat;
		this.sodium       = sodium;
		this.carbohydrate = carbohydrate;
	}
	
	public void main(String[] argus) {
		
		NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
	}

}
```

원하는 매개변수를 모두 포함한 생성자 중 가장 짧은 것을 골라 호출한다!

**단점**

 -사용자가 설정하길 원치 않는 매개변수까지 포함할 수 있다!

-매개변수 개수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다. 

-매개변수의 순서를 바꿔서 저장되는 경우 찾기 어려운 버그가 만들어 질 수 있다. 

- **자바빈즈 패턴**

```java
package item2.javabeans;

//자바빈즈 패턴 - 일관성이 깨지고, 불변으로 만들 수 없다. (16쪽)
public class NutritionFacts {
    // 매개변수들은 (기본값이 있다면) 기본값으로 초기화된다.
    private int servingSize  = -1; // 필수; 기본값 없음
    private int servings     = -1; // 필수; 기본값 없음
    private int calories     = 0;
    private int fat          = 0;
    private int sodium       = 0;
    private int carbohydrate = 0;

    public NutritionFacts() { }//빈 생성자
    
    // Setters 매서드 만들기 
    public void setServingSize(int val)  { servingSize = val; }
    public void setServings(int val)     { servings = val; }
    public void setCalories(int val)     { calories = val; }
    public void setFat(int val)          { fat = val; }
    public void setSodium(int val)       { sodium = val; }
    public void setCarbohydrate(int val) { carbohydrate = val; }

    
    public static void main(String[] args) {
        NutritionFacts cocaCola = new NutritionFacts();
        //setter 매서드를 통해 원하는 변수 값 설정
        cocaCola.setServingSize(240);
        cocaCola.setServings(8);
        cocaCola.setCalories(100);
        cocaCola.setSodium(35);
        cocaCola.setCarbohydrate(27);
    }

}
```

**단점**

-객체 하나를 만들기 위해 메서드를 여러 개 호출해야 한다. 

-객체가 완전히 생성되기 전까지 일관성이 무너진 상태에 놓이게 된다. 

## 자바로 구현하는 빌더 패턴

빌더 패턴

1. 클라이언트는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자(혹은 정적 팩터리)를 호출해 빌더 객체를 얻는다. 
2. 빌더 객체가 제공하는 일종의 Setter 메서드들로 원하는 선택 매개변수들을 설정한다. 
3. 매개변수가 없는 build 메서드를 호출해 필요한 객체를 얻는다. 

```java
package item2.Builder;
// Builder Pattern - 점층적 생성자 패턴과 자바빈즈 패턴의 장점만 취했다. (17~18쪽)
public class NutritionFacts {
	//변수들을 한곳에 모아둔다. 
	private final int servingSize;
	private final int servings;
	private final int calories;
	private final int fat;
	private final int sodium;
	private final int carbohydrate;

	public static class Builder {
		// Required parameters
		private final int servingSize;
		private final int servings;

		// Optional parameters - initialized to default values
		private int calories      = 0;
		private int fat           = 0;
		private int sodium        = 0;
		private int carbohydrate  = 0;

		//required Constructor
		public Builder(int servingSize, int servings) {
			this.servingSize = servingSize;
			this.servings    = servings;
		}
		
		
		//optional Setter method -> 메서드 체이닝을 위해 자신을 반환
		public Builder calories(int val){ 
			calories = val;      
			return this; //자신을 반환하여 연쇄 호출이 가능하다. 
			}
		
		public Builder fat(int val){ 
			fat = val;           
			return this;// 
			}
		
		public Builder sodium(int val){ 
			sodium = val;        
			return this; 
			}
		
		public Builder carbohydrate(int val){ 
			carbohydrate = val;  
			return this; 
			}

		public NutritionFacts build() {
			return new NutritionFacts(this);//최종적으로 객체를 만들어 주는 메서드를 추가한다. 
		}
	}

	private NutritionFacts(Builder builder) {
		servingSize  = builder.servingSize;
		servings     = builder.servings;
		calories     = builder.calories;
		fat          = builder.fat;
		sodium       = builder.sodium;
		carbohydrate = builder.carbohydrate;
	}

	public static void main(String[] args) {
		NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
				.calories(100).sodium(35).carbohydrate(27).build();
	}

}
```

빌더를 통해 메서드 체이닝을 위해 같은 객체로 반환한다. 

*구현할 때 중간에 디렉터를 두는 경우도 있다. → 빌더를 인터페이스화한다. 

디렉터를 사용하면 반복되는 빌더 호출을 디렉터에 숨겨놓고 클라이언트는 단순하게 받아다 쓸 수 있다. 

## 자바-스프링 에서 활용되는 예시

- 자바 8 Stream.Buidler API
    
    
- 롬복의 @Builder

```java
package com.domain;

import lombok.Builder;

public class NutritionFacts {
    private int servingSize;
    private int servings;
    private int calories;
    private int fat;
    private int sodium;
    private int carbohydrate;

    @Builder
    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
        this.servingSize  = servingSize;
        this.servings     = servings;
        this.calories     = calories;
        this.fat          = fat;
        this.sodium       = sodium;
        this.carbohydrate = carbohydrate;
    }

}
```

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package com.domain;

public class NutritionFacts {
    private int servingSize;
    private int servings;
    private int calories;
    private int fat;
    private int sodium;
    private int carbohydrate;

    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
        this.servingSize = servingSize;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }

    public static NutritionFactsBuilder builder() {
        return new NutritionFactsBuilder();
    }

    public static class NutritionFactsBuilder {
        private int servingSize;
        private int servings;
        private int calories;
        private int fat;
        private int sodium;
        private int carbohydrate;

        NutritionFactsBuilder() {
        }

        public NutritionFactsBuilder servingSize(int servingSize) {
            this.servingSize = servingSize;
            return this;
        }

        public NutritionFactsBuilder servings(int servings) {
            this.servings = servings;
            return this;
        }

        public NutritionFactsBuilder calories(int calories) {
            this.calories = calories;
            return this;
        }

        public NutritionFactsBuilder fat(int fat) {
            this.fat = fat;
            return this;
        }

        public NutritionFactsBuilder sodium(int sodium) {
            this.sodium = sodium;
            return this;
        }

        public NutritionFactsBuilder carbohydrate(int carbohydrate) {
            this.carbohydrate = carbohydrate;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this.servingSize, this.servings, this.calories, this.fat, this.sodium, this.carbohydrate);
        }

        public String toString() {
            return "NutritionFacts.NutritionFactsBuilder(servingSize=" + this.servingSize + ", servings=" + this.servings + ", calories=" + this.calories + ", fat=" + this.fat + ", sodium=" + this.sodium + ", carbohydrate=" + this.carbohydrate + ")";
        }
    }
}
```

- 스프링
• UriComponentsBuilder
• MockMvcWebClientBuilder

## 빌더 패턴의 장단점

### 장점

- 복합 객체 생성 과정을 캡슐화한다. → 복잡한 객체를 만드는 구체적인 과정을 숨길 수 있다.
- 불완전한 객체를 사용하지 못하도록 방지할 수 있다.

### 단점

- 팩토리를 사용할 때 보다 객체를 만들 때 클라이언트에 관해 더 많이 알아야 한다.
- 원하는 객체를 만들려면 빌더부터 만들어야 한다. → 객체를 조금 더 만들어야 한다…

## 참고

헤드퍼스트 디자인패턴
코딩으로 학습하는 GoF의 디자인 패턴 - 백기선
이펙티브 자바
소스 출처 :
-이펙티브 자바  [https://github.com/WegraLee/effective-java-3e-source-code/tree/master/src/effectivejava/chapter2/item2](https://github.com/WegraLee/effective-java-3e-source-code/tree/master/src/effectivejava/chapter2/item2)

