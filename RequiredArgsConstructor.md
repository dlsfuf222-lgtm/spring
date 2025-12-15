다음 문장은 Lombok의 @RequiredArgsConstructor 애노테이션에 대한 설명입니다. 문장별로 정리하여 설명드리겠습니다.

1. @RequiredArgsConstructor란?
@RequiredArgsConstructor는 필수(required) 필드만을 파라미터로 가지는 생성자를 자동으로 생성해주는 Lombok 애노테이션입니다.
여기서 말하는 필수 필드란 다음 두 가지입니다.
final로 선언된 필드
@NonNull 제약이 붙은 필드
즉, 객체 생성 시 반드시 값이 있어야 하는 필드만 생성자 인자로 포함됩니다.

2. “Required arguments are final fields and fields with constraints such as @NonNull.”
이 문장의 의미는 다음과 같습니다.
final 필드
→ 생성 시 반드시 초기화되어야 하므로 생성자 파라미터가 됩니다.
@NonNull 필드
→ null이 허용되지 않으므로 생성자에서 값이 전달되도록 강제됩니다.
→ 생성자 내부에 null 체크 코드도 함께 생성됩니다.
예시
```java
@RequiredArgsConstructor
@Component
public class OrderService {

    private final OrderRepository orderRepository;

    @NonNull
    private PaymentClient paymentClient;

    private int timeout; // 생성자에 포함되지 않음
}
자동 생성되는 생성자는 개념적으로 다음과 같습니다.
public OrderService(OrderRepository orderRepository,
                    PaymentClient paymentClient) {
    if (paymentClient == null) {
        throw new NullPointerException("paymentClient is marked non-null but is null");
    }
    this.orderRepository = orderRepository;
    this.paymentClient = paymentClient;
}
```

3. Spring의 @Component와 함께 사용하는 이유
Spring에서 의존성 주입(DI) 을 생성자 기반으로 사용할 때 매우 자주 쓰입니다.
```java
@Slf4j
@Component
@RequiredArgsConstructor
public class MemberService {

    private final MemberRepository memberRepository;
}
```
생성자는 Lombok이 자동 생성
Spring은 해당 생성자를 통해 의존성 주입
불변성(final) 유지 가능
테스트 및 유지보수에 유리
:point_right: Spring Boot 3 / 최신 Spring 권장 방식과 잘 맞습니다.

4. “Even though it is not listed, this annotation also has the onConstructor parameter.”
공식 요약 설명에는 잘 드러나지 않지만,
@RequiredArgsConstructor는 onConstructor 파라미터를 지원합니다.
이 파라미터를 사용하면 생성자에 추가 애노테이션을 붙일 수 있습니다.
예시
```java
@RequiredArgsConstructor(onConstructor_ = @Autowired)
public class SampleService {
    private final SampleRepository repository;
}
```
실제로는 다음과 같은 생성자가 만들어집니다.
```java
@Autowired
public SampleService(SampleRepository repository) {
    this.repository = repository;
}
```
※ Spring 4.3 이상에서는 단일 생성자일 경우 @Autowired가 필요 없으므로,
실무에서는 거의 사용되지 않습니다.

5. @Slf4j와의 관계
@Slf4j
Lombok이 private static final Logger log 필드를 자동 생성
생성자와는 직접적인 관련은 없지만,
보일러플레이트 코드 제거라는 공통 목적을 가짐

6. 핵심 요약
@RequiredArgsConstructor
final 또는 @NonNull 필드만 포함하는 생성자 생성
불필요한 필드는 제외
Spring에서 생성자 기반 DI와 매우 궁합이 좋음
onConstructor 파라미터로 생성자 애노테이션 추가 가능
불변 객체 설계 및 안정성 향상에 기여
필요하시다면 @AllArgsConstructor, @NoArgsConstructor와의 차이도 이어서 설명드릴 수 있습니다.
