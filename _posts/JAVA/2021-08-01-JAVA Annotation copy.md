---
title: JAVA Annotation
tags: JAVA
---

## Summry  

본 문서에서는 Java의 Annotation의 개념과 종류를 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Annotation?

Java 5부터 등장한 기능으로 흔히 알고 있는 @Override, @Deprecated이 대표적인 예다.  
AOP(Aspect Oriented Programing: 관심지향프로그래밍)을 편리하게 구성할 수 있다.  

Annotation은 사전을 찾아보면 "주석"이라고 나오지만, 우리가 흔히 사용하는 "//, /\**/" 등의 주석과는 차이가 있다.  
**어노테이션은 설명 그 이상의 역할을 한다. 어노테이션이 붙은 코드는 어노테이션의 구현된 정보에 따라 연결되는 방향이 결정된다.**  
따라서 전체 소스코드에서 비즈니스 로직에는 영향을 주지는 않지만 해당 타겟의 연결 방법이나 소스코드의 구조를 변경할 수 있다. 쉽게 말해서 "이 속성을 어떤 용도로 사용할까, 이 클래스에게 어떤 역할을 줄까?"를 결정해서 붙여준다고 볼 수 있다. **어노테이션은 소스코드에 메타데이터를 삽입하는 것이기 때문에 잘 이용하면 가독성 뿐 아니라 체계적인 소스코드를 구성하는데 도움을 준다.**  

어노테이션은 자바가 기본적으로 제공하기도 하고(@Deprecated, @Override, @SuppressWarnings), 개발자가 직접 정의해서 사용(Custom Annotation)할 수도 있다.  
이 기능을 잘 활용한다면, 비즈니스 로직과는 별로도 시스템 설정과 관련된 부가적인 사항들은 @에게 위임하고 개발자는 비즈니스 로직 구현에 집중할 수 있다.  

### 특징

어노테이션은 컴파일시기에 처리될 수도 있고 자바의 리플렉션을 거쳐서 런타임에 처리될 수도 있다.  

* 컴파일러에게 코드 문법 에러를 체크하도록 정보를 제공
* 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공
* 어노테이션을 만들 때 용도를 분명하게 해야 한다.
    * 소스상에서만 유지
    * 컴파일된 클래스에도 유지
    * 런타임 시에도 유지

## Annotation 종류

### Built-in Annotation

**@Override**  
* 메소드가 오버라이드 됐는지 검증.
* 만약 부모 클래스 또는 구현해야할 인터페이스에서 해당 메소드를 찾을 수 없다면 컴파일 오류 발생.

**@Deprecated**  
* 메소드를 사용하지 말도록 유도하며 만약 사용한다면 컴파일 경고를 발생.

**@SuppressWarnings**  
* 컴파일 경고를 무시.

**@SafeVarargs**  
* 제너릭 같은 가변인자 매개변수를 사용할 때 경고를 무시. (자바7 이상)

**@FunctionalInterface**  
* 람다 함수등을 위한 인터페이스를 지정하며 메소드가 없거나 두개 이상 되면 컴파일 오류 발생. (자바 8이상)

### Meta Annotation

> Meta Annotation을 활용해 Custom Annotation을 만들 수 있다.

**@Retention**
어노테이션의 Life Time.
* Class
    * 바이트 코드 파일까지 어노테이션 정보를 유지한다.
    * 하지만 리플렉션을 이용해서 어노테이션 정보를 얻을 수는 없다.
* Runtime
    * 바이트 코드 파일까지 어노테이션 정보를 유지하면서 리플렉션을 이용해서 런타임시에 어노테이션 정보를 얻을 수 있다.
* Source
    * Compile 이후로 삭제되는 형태

**@Inherited**  
* 어노테이션의 상속을 가능하게 한다.  

**@Documented**  
* 해당 annotation을 Javadoc에 포함.

**@Target**  
적용할 위치를 결정.  

**@Repeatable**  
반복적으로 어노테이션을 선언할 수 있게 한다.  

### Annotation 구조

```java
@Inherited // 상속
@Documented // 문서에 정보가 표현
@Retention(RetentionPolicy.RUNTIME) // 컴파일 이후에도 JVM에 의해서 참조가 가능합니다
@Retention(RetentionPolicy.CLASS)   // Compiler가 클래스를 참조할 때까지 유효합니다
@Retention(RetentionPolicy.SOURCE)  // 컴파일 이후 사라집니다
@Target({
		ElementType.PACKAGE, // 패키지 선언시
		ElementType.TYPE, // 타입 선언시
		ElementType.CONSTRUCTOR, // 생성자 선언시
		ElementType.FIELD, // 멤버 변수 선언시
		ElementType.METHOD, // 메소드 선언시
		ElementType.ANNOTATION_TYPE, // 어노테이션 타입 선언시
		ElementType.LOCAL_VARIABLE, // 지역 변수 선언시
		ElementType.PARAMETER, // 매개 변수 선언시
		ElementType.TYPE_PARAMETER, // 매개 변수 타입 선언시
		ElementType.TYPE_USE // 타입 사용시
})
public @interface TestAnnotation{
	/* enum 타입을 선언할 수 있습니다. */
	public enum Quality {
		BAD, GOOD, VERYGOOD
	}

	/* String은 기본 자료형은 아니지만 사용 가능합니다. */
	String value() default "TestAnnotation : Default String Value";

	/* 배열 형태로도 사용할 수 있습니다. */
	int[] values();

	/* enum 형태를 사용하는 방법입니다. */
	Quality quality() default Quality.GOOD;
}
```

## Custom Annotation 만들기

아래의 과정을 거쳐 어노테이션을 정의하고 사용할 수 있다.  
1. **@어노테이션 정의**
2. **@어노테이션을 타겟에 적용**
3. **@어노테이션을 사용해서 업무 수행**
4. **실행 완료**

어노테이션은 소스코드에 붙이는 하나의 라벨이라고 생각하면 이해가 쉽다.  
어노테이션을 직접 정의해서 사용하기 위해서는 먼저 어노테이션을 정의한 후, 원하는 타겟에 적용한다. 그 후 어노테이션이 붙은 타겟을 어떻게 사용할 지 기능을 구현하면, 해당 기능이 실행될 때 타겟에 붙은 어노테이션에 따라서 타겟의 경로가 결정된다.  

**간단한 Annotation 만들기**  
```java
public @interface TestAnno { 
    String value();
}

```
아주 심플한 커스텀 어노테이션이다. annotation 에서 선언한 method 이름은 아래처럼 annotation을 사용할 때 변수처럼 사용된다. 위의 경우처럼 method 가 한 개인 annotation 에서는 method 이름을 생략해서 사용할 수 있다.

```java
public class Test {
    @TestAnno(value="string")
    // @TestAnno("string") method 이름 생략
    public void doing(){

    }

}   
```

**default value**  
```java
public @interface TestAnno {
    String value() default "myWorld";
}
```
위처럼 default값을 할당할 수도 있으며 default 값이 할당 되어 있으면 아래처럼 인자를 넣지 않아도 사용할 수 있다.  

```java
public class Test {
    @TestAnno 
    public void doing(){

    }
}
```

### Simple Example

이 예제는 @StringInjector라는 간단한 커스텀 어노테이션을 만드는 예제이다.  
이 어노테이션은 멤버 변수에 선언시 해당 멤버 변수 타입이 String이라면 어노테이션에 정의된 값을 멤버 변수에 주입한다.  

**Annotation 선언**  
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * String 문자열을 주입하기 위해 선언하는 어노테이션입니다.
 * FIELD에만 선언가능하고 JVM이 어노테이션 정보를 참조합니다.
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface StringInjector {
    String value() default "This is StringInjector.";
}
```

**Annotation을 적용할 class**  
```java
public class MyObject {
    @StringInjector("My name is JDM.")
    private String name;

    @StringInjector
    private String defaultValue;

    @StringInjector
    private Integer invalidType;

    public String getName() {
        return name;
    }
    public String getDefaultValue() {
        return defaultValue;
    }
    public Integer getInvalidType() {
        return invalidType;
    }
}
```

**실제 Annotation을 찾고 주입하는 역할을 하는 class**  
```java
import java.lang.reflect.Field;

public class MyContextContainer {

    public MyContextContainer(){}

    /**
     * 객체를 반환하기 전 어노테이션을 적용합니다.
     * @param instance
     * @param <T>
     * @return
     * @throws IllegalAccessException
     */
    private <T> T invokeAnnonations(T instance) throws IllegalAccessException {
        Field [] fields = instance.getClass().getDeclaredFields();
        for( Field field : fields ){
            StringInjector annotation = field.getAnnotation(StringInjector.class);
            if( annotation != null && field.getType() == String.class ){
                field.setAccessible(true);
                field.set(instance, annotation.value());
            }
        }
        return instance;
    }

    /**
     * 매개변수로 받은 클래스의 객체를 반환합니다.
     * @param clazz
     * @param <T>
     * @return
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    public <T> T get(Class clazz) throws IllegalAccessException, InstantiationException {
        T instance = (T) clazz.newInstance();
        instance = invokeAnnonations(instance);
        return instance;
    }
}
```

**Test code**  
```java
public class AnnotationDemo {
    public static void main(String[] args) throws IllegalAccessException, InstantiationException {
        // 컨텍스트 컨테이너를 초기화 합니다.
        MyContextContainer demo = new MyContextContainer();

        // MyOjbect 객체를 하나 받아옵니다.
        MyObject obj = demo.get(MyObject.class);

        System.out.println(obj.getName()); // print is "My name is JDM."
        System.out.println(obj.getDefaultValue()); // print is "This is StringInjector."
        System.out.println(obj.getInvalidType()); // print is "null".
    }
}
```


## Reference

> [Java Annotation이란? - Nesoy](https://nesoy.github.io/articles/2018-04/Java-Annotation)  
> [ava Annotation: 인터페이스 강요로부터 자유를… - nextree](https://www.nextree.co.kr/p5864/)  
> [custom annotation 만들기 - 코드 다이버](https://codediver.tistory.com/71)