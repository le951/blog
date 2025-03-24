---
layout: post
title: Runtime Exception
date: 2025-03-24 08:32:00 +0900
description: # My First Post # Add post description (optional)
img: # software.jpg # Add image post (optional)
tags: [Exception, Runtime] # add tag
---


<br>
<hr>

### Exception

Exception : (일반적이지 않은) 예외

처리해서는 안되는 연산, 처리할 수 없는 연산, 정해진 무결성이 깨진 상황.

다 비슷한 말이다.\
일반적인 실행 흐름을 벗어난, 사전에 정의된 '예외'적인 상황.

그 때 Exception이 발생하고 처리된다.

<br>
<hr>

### Exception과 Error

Exception : 예외\
Error : 오류

예외는 대처할 수 있다.\
언제 어떻게 발생할지는 알 수 없더라도, 발생할 수 있다는 것이 예견되는 동시에 발생을 감지할 수 있다.

숫자를 0으로 나누지 마시오. Null인 Pointer를 호출하지 마시오.

코드 상 수정해야 하는 부분까지는 알 수 없더라도\
최소한 문제가 되는 이유를 이미 알고 정의하고 처리할 수 있다고 판단된 것이 예외다.

이 Exception들은 대체로 Java 언어 자체에 정의되어 있고, 필요하다면 추가로 정의할 수도 있다.

<br>

오류는 대처할 수 없거나, 예측할 수 없다.

ECC-RAM을 생각해보자.

정전기가 튀어 하드웨어적으로 메모리에 오류가 났다.\
Java가 이를 어떻게 처리하겠는가? 매 순간 모든 변수를 점검할 수는 없는 노릇 아닌가.

어느 날 업데이트로 JVM에 치명적인 결함이 생겼다면?\
이 또한 예측한다고 해결 될 문제가 아니다. JVM 자체가 고장났는데 JVM이 해결 가능할까? 확신할 수 없다.

외통수면 GG 쳐야지 붙들고 있는다고 묘수가 나오나.

이런 대처 불가능한 상황이 오류다. 

이 경우에 JVM은 예외처리를 통한 해결을 시도하지 않는다. 로그를 남기고 종료된다.

<br>
<hr>

### Exception과 If

if (db == null) return 0;

또는

try db.read() catch (NullPointException e) { throw e; }

둘 다 오류를 처리하는 방법이다.

장단점이 있다. 현대에는 병행해서 사용된다.


본래 먼저 사용되었던 것은 if를 통한 처리이다.

다만 if로 모든 오류를 처리한다는 것이 쉬운가.

코드가 길어지고 실행 흐름이 복잡해지고. 자연스레 읽기도 쓰기도 어려워진다.

그래서 이 오류를 처리하는 흐름과 코드를 아예 분리하자는 발상이 Exception이 되었고

분리하는 김에 사전에 정의도 해놓고. 언어 차원에서도 적극 활용하고. 여러가지를 거쳐 지금의 모습이다.


If와 try-catch가 오류를 처리하는 방식은 꽤나 다르다.

If는 매 실행마다 판별한다. 다만 아주 적은 성능을 요구한다. 수백 번 해도 우리가 마우스 한 번 휘젓는 것보다 적을만큼. 변수나 메소드를 읽어오는걸 제외하면 거의 cpu 1사이클이라고 한다.

try-catch는 판별하지 않는다. 그저 흘려보내다, 오류가 발생하면 그제서야 설명서(Exception table)를 참고하여 해결방법(catch)으로 이동한다.

그래서 try-catch는 실행되기 전까진 zero-cost라고 하지만, 동시에 한 번 작동하면 If와는 비교하기 힘들만큼 많은 자원을 사용한다. 중단하고, table을 참조하고, catch로 이동하고. 심지어 Exception이 throw 되는 것도 있다.

그 외에 오류의 처리를 호출한 위치에 넘겨버린다는 특징도 있다.


지금에선 이 둘을 병행하기도 한다.

오류가 예상되는 곳에는 가급적이면 If를 통해 처리한다.\
예상할 수 없었거나, 해결해야 하는 코드가 다른 장소에 있다면 throw하여 이 코드를 호출한 위치로 보낸다.

넘고 넘어 오류를 처리한다면 다시금 정상 흐름으로 돌리고\
처리할 수 없었다면 프로그램을 종료한다.

비슷하게 여러 패턴들이 있는 모양인데 아직은 잘 모르겠다.

<br>
<hr>

### try-catch-finally

Exception을 구현하기 위해 많은 언어에서 사용하는 방식.

try를 실행하고, Exception이 발생하면 대응하는 catch로 이동한다.\
처리가 모두 끝나면 (Exception이 발생 했든 안했든) finally로 이동한다.

이를 사용하지 않는 언어도 있다. error를 그대로 return하고, 프로그래머가 수동으로(또는 다른 라이브러리) 예외 처리한다거나. 이 경우에도 error를 Exception으로 만드는 꼴인 것은 같다.

<br>
<hr>

### Checked Exception

Java만의 개성.

Exception을 Compile time에 점검한다.\
예외가 처리되지 않으면 컴파일을 중단한다.

몰랐는데, 욕을 많이 들어먹는 방식이다.

Spring 같은 프레임워크들이 이걸 최소화 하고자 RuntimeException으로 래핑하기도 하고

사실상 한 집 사는 자식이나 다름없는 Kotlin 마저 이를 버렸다. (...)

Checked Exception이 무조건 나쁜 것인가? 그건 나는 잘 모르겠다.\
compile에 처리할 수 있는 오류는 강제한다는 발상 자체는 좋아보인다.

다만 조금만 많이 써도 지저분해지고, 귀찮아지는 것은 맞는 것 같다.

<br>
<hr>

### (Java Class) Exception과 RuntimeException 

다른 언어에서의 일반적인 예외처리가 Class RuntimeException.\
Checked Exception이 Class Exception.

public class Exception extends Throwable\
public class RuntimeException extends Exception

Throwable -> Exception -> RuntimeException 순으로 상속한다.

실제로 그냥 throw new Exception() 만 하면 컴파일이 불가능하다.\
Checked Exception 이기에 throws Exception 을 선언에 붙여주어야 한다.

그런데 RuntimeException은 throws를 하지 않아도 된다.\
상속한 Class가 이래도 되는건가? 싶다.

보통이라면 Throwable -> Exception -> Runtime / Checked 로 가는 구조가 맞지 않을까.\
어쩌면 이런 구조가 된 것은 Java의 시작이 Checked Exception 이었기 때문 아닐까?

라는 생각은 들었으나, 명확한 단서를 찾진 못했다.

Docs에 적힌 도입 시기는 같았다.

https://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html\
https://docs.oracle.com/javase/8/docs/api/java/lang/Exception.html\
https://docs.oracle.com/javase/8/docs/api/java/lang/RuntimeException.html

아마 Spec을 정하는 과정과 논의까지 찾아보아야 명확해질 것 같은데\
이건 미래의 내게로 미뤄두자.


