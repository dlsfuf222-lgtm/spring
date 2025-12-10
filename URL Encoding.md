URL Encode(Percent-Encoding)은 
### URI에서 허용되지 않는 문자나 특별한 의미를 가지는 문자를 안전하게 전달하기 위해 특정 형식으로 변환하는 과정입니다
아래에 핵심을 정리하였습니다.

> URL Encoding이 필요한 이유

HTTP 요청의 URL은 특정한 문자만 안전하게 사용할 수 있습니다.
 예를 들어 다음과 같은 문자들은 반드시 인코딩해야 합니다.
 * 공백()
 * %, ?, &, =, # 등의 예약 문자
 * ASCII 범위를 벗어난 문자(한글 등)

> URL Encoding 방식

### 문자를 %  + 16진수(HEX) 값으로 변환합니다.
예시:
문자 ASCII(HEX) URL-Encoded     공백( ) 20 '%20'   아 EC 95 84 '%EC%95%84'   / 2F '%2F'

1) 한글 인코딩
```java
"안녕하세요" → %EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94
```
2) 쿼리 파라미터 인코딩
```java
name=Tom & Jerry
```
URL 인코딩 후:
```
name=Tom%20%26%20Jerry
```
> 자바에서 URL 인코딩
Java 11+
```java
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;

String encoded = URLEncoder.encode("안녕하세요!", StandardCharsets.UTF_8);
System.out.println(encoded);
```
출력:
```java
%EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94%21
```
> URL Decoding
```java
import java.net.URLDecoder;

String decoded = URLDecoder.decode("%EC%95%88%EB%85%95", StandardCharsets.UTF_8);
```
