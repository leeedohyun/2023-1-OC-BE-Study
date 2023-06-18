# 람다와 스트림
## 람다
람다란 한 마디로 코드 블록이다. 기존의 코드 블록은 반드시 메서드 내에 존재해야 했지만, 자바 8부터는 람다를 메서드의 인자나 반환값으로 사용할 수 있게 되었다. 람다를 적용시킨 예제를 살펴보자.

<br>

```java
public class B003 {
  public static void main(String[] args) {
    Runnable r = () -> {
      System.out.println("Hello Lambda 3!!!");
    };
  
    r.run();
  }
}
```

이제 더이상 익명 객체를 만들지 않아도 된다. `->`가 추가되면서 람다의 구조를 `(인자 목록) -> {로직}`으로 표현할 수 있게 되었다. 위의 예제에서 봤듯이 함수형 인터페이스를 람다식으로 변경할 수 있다.
람다식을 함수형 인터페이스의 참조 변수에 저장해서 사용할 수도 있고 메서드의 인자로도 사용할 수 있다. 또한, 람다식을 메서드의 반환값으로 사용할 수도 있다. 

이뿐만이 아니라, 람다식은 다양한 용도가 있지만 그 중에서도 컬렉션 스트림을 위한 기능에도 사용할 수 있다. 더 적은 코드로 더 안정적인 코드를 작성할 수 있다. 예제를 살펴보자.

<br>

```java
public class B011 {
  public static void main(String[] args) {
    Integer[] ages = { 20, 25, 18, 27, 30, 21, 17, 19, 34, 28 };
    
    for (int i = 0; i < ages.length; i++) {
      if (ages[i] < 20) {
        System.out.format("Age %d!!! Can't enter\n", ages[i]);
      }
    }
  }
}
```

위의 예제는 20세 미만인 경우 출입을 금지하는 상황을 표현하는 코드이다. 위의 코드가 문제는 없지만 스트림을 통해서 더 간소화해 보자.


```java
public class B013 {
  public static void main(String[] args) {
    Integer[] ages = { 20, 25, 18, 27, 30, 21, 17, 19, 34, 28 };
    
    Arrays.stream(ages)
      .filter(age -> age < 20)
      .forEach(age -> System.out.format("Age %d!!! Can't enter\n", ages[i]));
  }
}
```

이렇게 스트림을 통해서 코드를 더 간략하게 작성할 수 있다. 스트림은 더 깊이 공부하는 시간을 가지도록 하자.

# 옵셔널
자바의 `NullPointException` 문제를 해결할 수 있는 방법을 제공한다.
