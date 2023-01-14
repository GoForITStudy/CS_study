## BigDecimal

### Double 연산의 문제점
```
@Test 
@DisplayName("double 연산 테스트") 
fun doubleCalcTest() { 
	val a = 100.00000000005 
	val b = 10.00000000004 
	Assertions.assertThat(a-b).isEqualTo(90.00000000001) 
}
```

위의 코드를 단위 테스트를 하게되면 `expected: 90.00000000001 but was: 90.00000000000999`  부동 소수점 에러가 발생한다.
자바에서 double 은 정확한 실수를 저장하지 않고 근사치 값을 저장하기 때문에 정확한 값을 계산하기 위해서 double 이 아닌 BigDecimal 을 사용해야한다. 

### BigDecimal
크고 정확한 실수 값을 저장하기 위해서 BigDecimal 을 사용해야한다.

#### 1. BigDecimal 의 멤버변수
* intVal (BigInteger) : 정수
- scale (int) : 소수점 이후의 자리 수
- percision (int) : 해당 숫자의 총 자리 수
- intCompact (int) : BigDecimal 의 길이가 18자리 이하일 때 intVal 변수에 저장하지 않고 intCompact 에 저장

#### 2. BigDecimal 사용지 주의할 점
Double 을 BigDecimal 로 변환할 때 주의해야한다.
```
@Test 
@DisplayName("bigDecimal Test 2") 
fun bigDecimalTest2() { 
	val num = 0.1 
	println(BigDecimal(num)) 
}
```

double 을 바로 BigDecimal 에 바로 대입하면 예상했던 값이 나오지 않는다. 0.1 이 출력될 거라고 예상했지만 0.1000000000000000055511151231257827021181583404541015625 이 출력되었다.
double 은 정확한 지수 표현이 아니기 때문에 정확한 계산을 위해서 다음과 같이 사용해야한다. 

```
@Test 
@DisplayName("bigDecimal Test 3") 
fun bigDecimalTest3() { 
	val num = 0.1 
	println(BigDecimal.valueOf(num)) 
}
```

### BigDecimal 의 연산
BIgDecimal 은 String 과 같이 불변객체라 연산 후 반환 타입이 BigDecimal 인 경우 새로운 인스턴스를 반환하게된다.

##### 1. add(val: BigDecimal)
```
@Test 
@DisplayName("bigDecimal Test 4") 
fun bigDecimalAddTest() { 
	// val num1 = BigDecimal(10.0000000001) 로 하면 double 연산이 되어 정확한 
	// 값이 저장되지않음 
	val num1 = BigDecimal("10.00000000001") 
	val num2 = BigDecimal("10.00000000002") 
	println(num1.add(num2)) 
	println(num1 + num2) 
}
```

##### 2. substract(val: BigDecimal)
```
@Test 
@DisplayName("BigDecimal Test 5") 
fun bigDecimalSubstractTest() { 
	val num1 = BigDecimal("100.010") 
	val num2 = BigDecimal("100.005") 
	println(num2.subtract(num1)) 
}
```

##### 3. multiply(val: BigDecimal)
```
@Test 
@DisplayName("BigDecimal Test 6") 
fun bigDecimalMultiply() { 
	val num = BigDecimal("100.001") 
	val num2 = BigDecimal("50.001") 
	println(num.multiply(num2)) 
}
```

##### 4. divide(val: BigDecimal)
```
@Test 
@DisplayName("BigDecimal Test 7") 
fun bigDecimalDivide() { 
	val num = BigDecimal("100.001") 
	val num2 = BigDecimal("50.001") 
	println(num.divide(num2, 5, RoundingMode.CEILING)) 
}
```

실수를 나눌 때 나머지가 끝도없을 수 있다. divide 메서드는 RoundingMode 를 통해 어떻게 반올림할지, 몇번째 자리에서 반올림 할 지 지정할 수 있다. 결과가 정확하게 떨어지지 않는 경우 나머지에 대한 처리를 지정해야한다는 에러가 발생할 수 있다. `Non-terminating decimal expansion; no exact representable decimal result.` 

### BigDecimal 의 비교연산
#### 1. equals 를 이용한 동등 비교
```
@Test 
@DisplayName("BigDecimal 의 동등비교") 
fun bigDecimalEquals() { 
	val num1 = BigDecimal("0.05") 
	val num2 = BigDecimal("0.05") 
	if(num1.equals(num2)) { 
		println("hello") 
	} 
}
```

BIgDecimal 을 동등비교 할 때 == 를 사용하여 비교할 생각을 할 수 있다. == 는 두 객체가 같은지 비교하는 연산이지 값이 같은지 비교하는 연산이 아니다. 값을 비교하기 위해서는 equals 메서드를 사용해야한다.

#### 2. compareTo를 이용한 크기 비교
Wrapper 타입인 Long, Double은 primitive 타입으로 자동 형변환이 되기 때문에 비교 연산자로 크기를 비교할 수 있다. BigDecimal 은 자동 형변환을 하지않아서 비교 연산자로 비교할 수 없다. BigDecimal 은 Comparable 인터페이스를 구현하고 있어 compareTo를 사용해야한다.

```
@Test 
@DisplayName("BigDecimal 의 비교연산") 
fun bigDecimalCompareTo() { 
	val num1 = BigDecimal("0.05") 
	val num2 = BigDecimal("0.04") 
	when(num1.compareTo(num2)) { 
		-1 -> println("num1 은 num2 보다 작다") 
		0 -> println("num1 은 num2 와 같다") 
		1 -> println("num1 은 num2 보다 크다") 
		} 
	}
}
```

compareTo의 결과가 -1 라면 비교 대상보다 작고, 0 이라면 비교 대상과 같고, 1 이라면 비교 대상보다 크다는 의미다.

#### BigDecimal 의 절대값 및 복합연산
```
@Test 
@DisplayName("계산") 
fun calcRateOfIncreaseTest() { 
	val todayPrice = BigDecimal.valueOf(30680000.00000000) 
	val yesterDayPrice = BigDecimal.valueOf(30685000.00000000) 
	println("----------before----------") 
	println("증가율 ${calcRateOfIncrease(todayPrice, yesterDayPrice).abs()}") 
	println("----------after----------") 
	val divideResult = (todayPrice.minus(yesterDayPrice))
	.divide(todayPrice, 10, RoundingMode.HALF_UP).multiply(BigDecimal("100")) 
	println("minus : ${todayPrice.minus(yesterDayPrice).toPlainString()}") 
	println("나누기 결과 : $divideResult") 
}
```

절대값을 구하기 위해서는 abs() 를 뒤에 붙여주기만 하면 된다. minus 보다 multiply 가 우선순위가 커서 multiply 연산이 먼저 될 수 있는데 minus 를 먼저 하고싶은 경우에는 괄호로 묶어주면 된다.