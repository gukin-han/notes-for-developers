# Sec 3: Working with Text

> String은 모든 곳에 가장 기본적으로 사용되기 때문에 중요하다.
> 

## Creating Strings

> String을 생성하는데 두 가지 방법이 소개된다.  큰 따옴표를 이용하여 할당하는 기존의 방법과 new operator를 이용한 방법의 차이점에 대해 설명할 수 있어야 한다.
> 
- create new project with name of People
    - 참고로 Java 16 아래의 버전은 권장하지 않는다.
- Open Project with New Window
    - 이전에 사용했던 project로 jump back할 수 도 있기 때문이다.
    - It’s up to you.
- Create playground where we can try something out.
    - with name of LearnStrings.
- create String
    - fruit variable of String with the value of “apple”
    - use new method to create String

```java
String fruit = "apple";
String anotherFruit = "apple";
String vegetable = new String("broccoli");
String anotherVegetable = new String("broccoli");
```

- lesson
    - we can create String with various way (e.g., new operator)
    - `String fruit = "apple";`
        - OS에 새로운 메모리를 요청하고 메모리 주소를 돌려받는다.
        - 그 메모리 주소에 값이 할당되고, `fruit`변수가 그 메모리 주소를 가리킨다.
    - `String anotherFruit = "apple";`
        - 이전의 history를 찾아보고 동일한 값을 가지고 있다면,
        - **같은 메모리 주소를 참조**하게 한다.
    - `==` operator를 이용해 변수 내의 값을 비교할 수 있다. (이 경우에는 pointer를 비교한다.)
        
        ![스크린샷 2023-03-06 오후 4.47.19.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.47.19.png)
        
    - `String vegetable = new String("broccoli");`
        - 위와 동일하게 새로운 메모리를 요청하여 메모리 주소를 돌려받는다.
        - 그 메모리 주소에 값이 할당되고, `vegetable`변수가 그 메모리 주소를 가리킨다.
    - `String anotherVegetable = new String("broccoli");`
        - **새로운 메모리를 요청하여, 새로운 주소값을 가진다.**
    
    ![스크린샷 2023-03-06 오후 4.27.57.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.27.57.png)
    

## Upper & Lower Casing

> 문자의 정규화(normalize)를 할 필요성이 있다. 보통 String을 유저의 input으로 부터 받게 되면 알파벳의 경우 대문자나 소문자, 그리고 대소문자 혼합된 방식 등 여러가지의 경우의 수가 있다. 예를 들어, 문자열 match와 같은 논리 연산을 아무런 에러 없이 하려면 입력 받은 문자열을 정규화하여  consistence를 유지할 필요가 있다.
> 
- create variable myText of String with some value;
- String class에는 위의 이슈들을 해결할 수 있는 여러 method가 존재한다.

![스크린샷 2023-03-06 오후 4.41.16.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.41.16.png)

## Strings: `isBlank` or `isEmpty`?

> 여기서는 비슷하지만 다른 두 가지 method에 대해서 다룬다. `isEmpty`와 `isBlank`. 둘의 차이점을 설명할 수 있고, 언제 사용하면 좋을지에 대해 알아야한다.
> 
- `isEmpty` return boolean
    - 이 메소드의 경우 하나의 character라도 가지고 있으면 false, 그렇지 않으면 true를 돌려준다.

![스크린샷 2023-03-06 오후 4.56.15.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.56.15.png)

- 참고로, space는 character에 속한다.
    - 각 character는 개별적인 유니코드를 가지는데 space 또한 가지고 있다.
    
    ![스크린샷 2023-03-06 오후 4.54.45.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.54.45.png)
    
- `isBlank`
    - whitespace(공백)와 no space 모두 true를 반환한다.
    
    ![스크린샷 2023-03-06 오후 5.01.10.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-06_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.01.10.png)
    
- name과 같은 user input을 받는 경우가 있다.
- 만약 이름을 받아 `capitalize`를 수행하려고 하는데, blank가 존재하면 문제가 생긴다.
- 따라서, 이 user input에 blank나 empty의 상황이 있을 경우 문제가 생기는 경우를 막기 위해,
- safeguard 용도로 사용한다.

## Replacing Text within Strings - `replace()`

> String 내부의 text 요소를 바꾸는(replace) 방법에 대해 다룬다.
> 
- 임의의 String을 만든다.
- search and replace
- `replace()`
    
    ![스크린샷 2023-03-07 오후 2.35.04.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.35.04.png)
    
    - parameter로 CharSequence라고 되어있는데, 일단 String이라고 생각하면 된다.
        - search하려는 target과 replace하려는 replacement로 이뤄져있다. (`command` + `p`)
        - oldChar와 newChar도 있다.

```java
String myText = "here's firf my awesome firf text";
System.out.println(myText);
System.out.println(myText.replace("firf", "nice"));
```

![스크린샷 2023-03-07 오후 2.46.48.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.46.48.png)

```java
String myText = "here's firf my awesome firf text";
System.out.println(myText);
System.out.println(myText.replace('e', 'z'));
```

![스크린샷 2023-03-07 오후 2.47.21.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.47.21.png)

- 여러 요소들에 대해 replace가 이뤄졌다.
- case sensitive한가? yes.

## Removing white space - `strip()`

> strip , trim remove whitespace 처음과 끝, user input 유저가 실수로 혹은 의도적으로 space를 입력하는 경우, 제대로된 input값을 받아 처리하기 위해(normal). 나중에 space가 문제가 생길 수 있기 때문에 사용하는 경우가 많다. 여기서는 `strip()`의 사용 방법과 `trip()`과의 차이점에 대해 설명한다.
> 

### `strip()`

```java
String firstName = "  Jake  ";
System.out.format("'%s'", firstName);
System.out.format("'%s'", firstName.strip());
System.out.format("'%s'", firstName.stripLeading());
System.out.format("'%s'", firstName.stripTrailing());
```

![스크린샷 2023-03-07 오후 2.53.50.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.53.50.png)

- leading space and trailing space라고 부른다.
    - `strip()` 메소드를 이용하면 leading space와 trailing space 모두 제거한다.
    - `stripLeading()` 메소드를 이용하면 leading space만 제거한다.
    - `stripTrailing()` 메소드를 이용하면 trailing space만 제거한다.

### Multi-line String에 관하여

> 다른 언어와 다르게 Java에서는 Multi-line String이 늦게 사용 가능해졌다. 큰 따옴표 세 개를 이용해 아래와 같이 Multi-line String을 구현할 수 있다.
> 

![스크린샷 2023-03-07 오후 3.02.18.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.02.18.png)

```java
String firstName = """
    <html>
        <div>here's a div block</div>
    </html>
    """;
System.out.format("'%s'", firstName.stripIndent());
```

![스크린샷 2023-03-07 오후 3.04.54.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.04.54.png)

![스크린샷 2023-03-07 오후 3.05.23.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.05.23.png)

![스크린샷 2023-03-07 오후 3.07.10.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.07.10.png)

- 위 코드의 실행 결과는 동일하다. 위에서 보이는 green line이전의 indent는 IDE에서 해결을 해준다.
- `stripIndent()`에 대해 설명을 해주었는데, 요점을 잘 모르겠다. 내 생각으로는 위 불렛 포인트 내용과 동일하게 IDE에서 자동으로 Indent에 대해 strip하는 기능이 있기 때문에 메소드 사용하기 전 후가 다름이 없다는 것을 얘기 하려는 것으로 보인다.

### `trim()`

- `strip()`보다 오래된 방법
    - 오래된 java버전에서는 `strip()`은 존재하지 않기 때문에 `trim()`을 사용한다.
    - 이전 section에서도 다뤘듯이 whitespace의 기준이 나라마다 다르기 때문에 알파벳의 whitespace에서 문제 없이 작동하는 특정 코드가 다른 언어의 whitespace에서 문제가 발생할 수 있다.
    - 이 처럼 `trim`보다는 `strip`이 좀 덜 language sensitive한 방법이며 advanced한 방법이다.
    - 결론은 strip이 사용 가능하면 strip을 사용하고 그렇지 않으면 trim을 사용한다.
    
    ```java
    String firstName = "  Jake  ";
    System.out.format("'%s'", firstName);
    System.out.format("'%s'", firstName.trim());
    ```
    
    ![스크린샷 2023-03-07 오후 3.14.08.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.14.08.png)
    
- 만약 Strip을 구현해본다면
    - 첫 번째 방법
        
        ```java
        public static String strip(String text) {
            String result = "";
            for(int i = 0; i < text.length(); i++) {
                if (text.charAt(i) != ' ') {
                    result += text.charAt(i);
                }
            }
            return result;
        }
        ```
        
    - 두 번째 방법
        
        ```java
        public static String strip(String text) {
            return text.replace(" ", "");
        }
        ```
        
        - 위 두 방법은 issue가 있다. 예를 들어, tap과 같이 우리가 whitespace라고 생각하지만 실제로는 그렇지 않는 char들이 존재한다. 이러한 것들에 대해 우리가 원하는 결과를 얻을 수 없다.

## Getting individual characters of  a string - `charAt()`

> 여기서는 String class의 `charAt()` 메소드에 대해 다룬다.
> 

```java
String myText = "Apples";
System.out.println(myText.charAt(3));
```

- index를 기준으로 String에서의 char를 가져온다.
- index는 0부터 시작한다.
- 이를 주의해서 사용하고, index를 넘어가는 값을 요구하면 Exception (=error)를 발생시킨다.
    
    ```java
    String myText = "Apples";
    
    int length = myText.length();
    if (length > 9) {
        System.out.println(myText.charAt(9));
    }
    ```
    
    - 이러한 에러를 방지하는 간단한 방법.
- 유저로 부터 first name, middle name, last name을 받는 경우가 있다.
    - 이 경우 보통 middle name을 전부 받지 않고 이니셜만 받을 때 사용할 수 있다.

```java
String middleName = "Christopher";
System.out.println(middleName.charAt(0));
```

![스크린샷 2023-03-07 오후 3.50.57.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.50.57.png)

## Comparing String for Alphabetical Order `compareTo()`

> object의 order를 비교하여 negative integer, 0, positive integer중 하나를 return한다. alphabet뿐만 아니라 다른 언어도 사전적 순서에 의해 비교할 수 있다.
> 

### `comareTo()`

```java
String firstWord = "가";
String secondWord = "나";

System.out.println(firstWord.compareTo(secondWord));
```

### `compareToIgnoreCase()`

- `compareTo()` 메소드와 앞에서 배운 `toUpperCase()`, `toLowerCase()` 메소드를 이용해서 `compareToIgnoreCase()` 메소드를 구현해보기.

```java
public static int compareToIgnoreCase(String text1, String text2) {
    String lowerText1 = text1.toLowerCase();
    String lowerText2 = text2.toLowerCase();
    return lowerText1.compareTo(lowerText2);
}
```

## Determining if Text is Contained in a String - `contains()`

> 유저가 입력한 String에서 특정 키워드를 포함하는지 확인하는 용도로 사용할 수 있다.
> 

```java
String myText = "Four score and seven years ago";
System.out.println(myText.contains("seven"));
```

![스크린샷 2023-03-07 오후 4.19.35.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.19.35.png)

- 여기서도 case sensitive하다.

## String Concatenation - `concat()`

> String을 붙이는 여러가지 방법에 대해 다룬다.
> 

```java
String text1 = "this is my text1";
String text2 = "this is my text2";
System.out.println(text1 + " " + text2);
```

![스크린샷 2023-03-07 오후 4.27.52.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.27.52.png)

### 만약 loop를 돌려야 한다면?

- 위와 같은 + sign을 사용하지 않는게 좋다. 매 순간 object가 새로 생기기 때문에 memory 낭비가 심각하기 때문이다.
    - 당장은 이것의 해결 방법을 알 필요는 없지만, 위에 사실에 대해 유의하면 좋다.

```java
String text1 = "this is my text1";
String text2 = "this is my text2";
System.out.println(text1.concat(text2));
System.out.println(text1.concat(" " + text2)); // looks bad, really bad
System.out.println(text1.concat(" ".concat(text2))); // looks bad, really bad
```

### “” literal에 대하여

```java
System.out.println("my string literal".concat(text1));
```

- 위 처럼 “” 큰 따옴표에 있는 literal을 String object 처럼 사용할 수 있다. 즉, 모든 method를 사용가능하다.

### `StringBuilder()` 클래스

> 긴 String을 붙일 수 있게 설계되어있다.
> 

```java
String text1 = "this is my text1";
String text2 = "this is my text2";

String finalString = new StringBuilder().append(text1).append(text2).toString();
System.out.println(finalString);
```

- 스케치북, 여러 String을 붙일때 메모리 효율적인 방식
- append 메소드 return `StringBuilder`
- 마지막에 `toString()`을 붙인다.
    - override 버전
    - 이를 붙이지 않으면 `StringBuilder`를 돌려주기 때문에 String 타입의 변수에 저장할 수 없다.
- 다음과 같이 가독성을 높일 수 도 있다.

```java
String finalString = new StringBuilder()
        .append(text1)
        .append(text2)
        .toString();
```

- constructor에게 얼마나 긴 String을 가질 수 있을지 전달할 수 있다.

```java
String finalString = new StringBuilder(text1.length() + text2.length() + 1)
        .append(text1)
        .append(" ")
        .append(text2)
        .toString();
```

- 만약 할당된 메모리를 초과하여 rellocate을 하게된다면 속도가 잠깐 느려질 수 있다.

### `StringBuffer()` 클래스

> `StringBuilder()`와 굉장히 유사
> 

```java
String finalString = new StringBuffer()
        .append(text1)
        .append(" ")
        .append(text2)
        .toString();
```

- StringBuffer()가 오래되었고 약간의 이슈가 있다.
    - threads에서 문제 (thread safe) → 속도가 느림
    - 하지만 thread가 크게 문제가 안되기 때문에 StringBuilder()를 만듬

### 다른 방법

```java
System.out.format("%s %s\n", text1, text2); // format does not make a new line
```

- 만약 추가적으로 String을 붙이고 싶다면, 다음과 같이  String에 저장 후 재 사용.

```java
String oneMoreFinalString = String.format("%s %s", text1, tex2);)
```

## Determining the Length of a String - `length()`

```java
String myText = "for";
System.out.println(myText.length());
```

- length() 메소드가 필요한 이유
    - 예제1
        - String을 char array로 바꿔서 (`toCharArray()`) 사용하고 싶은 경우가 있다.
        - 이후 이 array에 index를 기준으로 접근하려고 할때, index limit을 초과하는 index를 받았을 경우에 safe guard용으로 사용할 수 있다.
        - 아래 코드를 참조
            
            ```java
            String myText = "for";
            System.out.println(myText.length());
            char[] chars = myText.toCharArray();
            
            int index = 3;
            if (index < myText.length() && index >= 0 {
                System.out.println(chars[index]);
            }
            ```
            
            - 초과하는 index를 넣어줘도 error를 발생시키지 않게 할 수 있다.
    - 예제2
        - 전 section에서 설명함.
        - 메모리 super super 효율성을 위한 방법으로 `StingBuilder`의 argument로 length()값을 넣을 때 사용할 수 있다.
        - 참고로, 아무런 argument가 없는 경우 default값을 넣어준다.
        
        ```java
        String myText = "for";
        String secondText = "score";
        StringBuilder builder = new StringBuilder(myText.length() + secondText.length()).append(myText).append(secondText);
        ```
        

## 36. Getting Parts of a String - `substring()`

> get part of the entire string; String에서 일부의 String을 가져오는 방법이며, index를 기준으로 작동한다.
> 

![스크린샷 2023-03-07 오후 5.27.17.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.27.17.png)

- 위 처럼 두가지 방법이 있다.
    - 첫 번째 방법은 시작 index를 주고 끝까지 substring
        
        ```java
        String myText = "apple";
        String myNewText = myText.substring(0);
        System.out.println(myNewText);
        ```
        
        ![스크린샷 2023-03-07 오후 5.28.23.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.28.23.png)
        
    - 두 번째 방법은
        - end index는 exclusive라는 점을 인지.
        
        ```java
        String myText = "apple";
        String myNewText = myText.substring(0, 1); // length of 1 (character)
        System.out.println(myNewText);
        ```
        
        ![스크린샷 2023-03-07 오후 5.30.25.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.30.25.png)
        
    - 만약 apple을 caplitalize하고 싶다면?
        - 첫 번째 방법
            
            ```java
            String myText = "apple";
            String myNewText = myText.substring(0, 1).toUpperCase() + myText.substring(1);
            System.out.println(myNewText);
            ```
            
            ![스크린샷 2023-03-07 오후 5.33.54.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.33.54.png)
            
        - 두 번째 방법
            
            ```java
            String myText = "apple";
            String myNewText = myText.substring(0, 1).toUpperCase().concat(myText.substring(1));
            System.out.println(myNewText);
            ```
            
        - 세 번째 방법 - make it more readable.
            - `command` + `option` + `v` (as in Variable)
            
            ```java
            String myText = "apple";
            String firstPart = myText.substring(0, 1);
            String secondPart = myText.substring(1);
            String capitalized = firstPart.toUpperCase();
            String myNewText = capitalized.concat(secondPart);
            System.out.println(myNewText);
            ```
            
            ![스크린샷 2023-03-07 오후 5.42.30.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.42.30.png)
            
            - 우리가 이해할 수 있는 bite 사이즈의 크기로 쪼개는 행위를 장려하는 편이다. (refactoring)
            - come up with the meaningful names
            - 나중에 생각해도 된다.
            
            ```java
            String myText = "apple";
            String firstPart = myText.substring(0, 1);
            String secondPart = myText.substring(1);
            String capitalFirstLetter = firstPart.toUpperCase();
            String myNewText = capitalFirstLetter.concat(secondPart);
            System.out.println(myNewText);
            ```
            
            - `fn` + `shift` + `f6` → 변수 이름을 한번에 바꾸는 방법
            - 만약 `Stringbuilder`를 이용한다면?
            
            ```java
            String myText = "apple";
            String firstPart = myText.substring(0, 1);
            String secondPart = myText.substring(1);
            String capitalFirstLetter = firstPart.toUpperCase();
            
            StringBuilder builder1 = new StringBuilder()
                    .append(capitalFirstLetter)
                    .append(secondPart)
                    .toString();
            
            StringBuilder builder2 = new StringBuilder(capitalFirstLetter)
                    .append(secondPart)
                    .toString();
            
            //hyper efficient way to concat
            int totalLength = capitalFirstLetter.length() + secondPart.length();
            StringBuilder builder1 = new StringBuilder(totalLength)
                    .append(capitalFirstLetter)
                    .append(secondPart)
                    .toString();
            
            System.out.println(builder1);
            ```
            
            ![스크린샷 2023-03-07 오후 5.50.53.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.50.53.png)
            
            - `fn` + `shift` + `f6` 을 누르면 자동으로 capacity를 변수 이름으로 잡는데 위에서 originate

## 37. Searching within a String - `indexOf()`

> 많이 사용되는 method이며, 매우 유용하다.
> 

### `indexOf()`

- seven이 존재하는지 그리고 어디에 있는지를 동시에 알고 싶다.
- `indexOf`를 사용한다.

```java
String myText = "four score and seven years ago";
System.out.println(myText.indexOf("seven"));
```

- 예제를 쉽게 해본다.
    - 만약 integer를 넣으면? find the char encoded as 65 (uni code)
        
        ```java
        String myText = "ABCDEFGABCDEFG";
        System.out.println(myText.indexOf(65)); // 65는 A
        ```
        
        ![스크린샷 2023-03-07 오후 6.27.00.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_6.27.00.png)
        
    - 가장 첫 번째로 match되는 단어의 index를 돌려준다.
    - 이렇게 integer를 이용해서 찾는 이유는 whitespace와 같이 시각적으로 표현해서 타이핑해넣기 어려운 char들을 찾는데 굉장히 유용하기 때문이다.

> 📝 character를 integer로 encode하는 시스템이 존재하며, 이를 unicode라고 한다.
> 
- indexOf()를 사용하는 여러가지 방법이 있다.
    
    ![스크린샷 2023-03-07 오후 6.31.49.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_6.31.49.png)
    
- -1을 돌려주면 입력값이 존재하지 않는다는 의미.
- case sensitive함.

### `lastIndexOf()`

```java
String myText = "ABCDEFGABCDEFG";
System.out.println(myText.lastIndexOf("A"));
```

![스크린샷 2023-03-07 오후 6.35.24.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_6.35.24.png)

### 시작점을 지정 - int fromIndex

```java
String myText = "ABCDEFGABCDEFG";
System.out.println(myText.indexOf("A", 2));
```

### Challenge

![스크린샷 2023-03-07 오후 6.43.04.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_6.43.04.png)

```java
public static String parseAreaCode(String phoneNumber) {
    return phoneNumber.substring(phoneNumber.indexOf("(")+1, phoneNumber.indexOf("("));
}

public static String parseExchage(String phoneNumber) {
    return phoneNumber.substring(phoneNumber.indexOf(" ")+1, phoneNumber.indexOf("-"));
}

public static String parseLineNumber(String phoneNumber) {
    return phoneNumber.substring(phoneNumber.indexOf("-")+1);
}
```

```java
public static String parseAreaCode(String phoneNumber) {
    int openParens = phoneNumber.indexOf("(");
    int closeParens = phoneNumber.indexOf(")");
    String areaCode = phoneNumber.substring(openParens + 1, closeParens);
		return areaCode;
}

public static String parseExchange(String phoneNumber) {
    int spaceIdx = phoneNumber.indexOf(" ");
    int hyphenIdx = phoneNumber.indexOf("-");
    String exchange = phoneNumber.substring(spaceIdx + 1, hyphenIdx);
		return exchange;
}

public static String parseLineNumber(String phoneNumber) {
    int hypenIdx = phoneNumber.indexOf("-");
		String lineNumber = phoneNumber.substring(hypenIdx + 1);
    return lineNumber;
}
```

- index의 exclusive와 inclusive는 항상 confusing하니 유의해서 작성할것

> parse라는 단어에 대해
> 
> 
> ![스크린샷 2023-03-07 오후 6.59.00.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-07_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_6.59.00.png)
> 
- regular expression에 대해 배우면 좀더 쉽게 할 수 있다.

## 38. Splitting String Apart - `split()`

- 실무에서는 comma separated file(CSV) 을 이용한다.
- `split()`을 사용할때, regular expression(regex)을 사용한다.

```java
String text = """
				Smith,Fred,1/1/79,1111 ABC St.,Apple,CA
				McGuire,Jerry,2/2/80,2222 DEF St.,Orange,NV
				""";
String[] people = text.split("\n");
String[] fred = people[0].split(",");
```

## **39. Beginning & Ending of Strings - `startsWith()` & `endsWith()`**

> 예를 들어, user가 입력한 String에서 extension을 찾을때 사용할 수 있다. String을 받아 boolean을 돌려준다.
> 

```java
String filename = "myfile.txt";
System.out.println(filename.endsWith("txt"));
```

![스크린샷 2023-03-08 오전 12.19.09.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.19.09.png)

```java
String filename = "file001.txt";
System.out.println(filename.startsWith("file"));
```

![스크린샷 2023-03-08 오전 12.20.27.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.20.27.png)

```java
String filename = "   file001.txt".strip();
System.out.println(filename.startsWith("file"));
```

```java
String filename = "   file001.txt";
String strippedString = filename.strip();
System.out.println(strippedString.startsWith("file"));
```

- strip()은 new String을 돌려준다.

![스크린샷 2023-03-08 오전 12.26.54.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.26.54.png)

## **40. Comparing Strings for Equality - `contentEquals()`**

```java
String firstText = "Apple";
String secondText = "Apple";

System.out.println(firstText.contentEquals(secondText));
```

![스크린샷 2023-03-08 오전 12.29.50.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.29.50.png)

> 참고로, contentEquals() method는 String이 아닌 boolean을 return하는데, 그럼에도 불구하고 println() 메소드는 boolean 값을 받아 출력한다. 그 이유의 까닭은 아래와 같다.
> 
> 
> ![스크린샷 2023-03-08 오전 12.32.57.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.32.57.png)
> 
> ![스크린샷 2023-03-08 오전 12.33.10.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.33.10.png)
> 
> ![스크린샷 2023-03-08 오전 12.33.36.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.33.36.png)
> 

### `equals()`

- 차이점은?
    - `equals()`은 굉장히 strict하다 → String 만 받음.
    - 그에 비해 `contentEquals()` 좀더 flexiable함. → “content”라는 단어를 보면 이해에 도움이 된다.
- 예를 들어,
    
    ![스크린샷 2023-03-08 오전 12.39.40.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.39.40.png)
    
    - String과, StringBuffer 둘다 characters sequence이다.

## Exercises

### Ex1.

> Model a Person with a first name and last name and ensure that even if the first name is entered all lowercase, it will be stored all uppercase.
> 

### Ex2.

> Wreti
> 

![스크린샷 2023-03-08 오전 12.54.49.png](src/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-08_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.54.49.png)