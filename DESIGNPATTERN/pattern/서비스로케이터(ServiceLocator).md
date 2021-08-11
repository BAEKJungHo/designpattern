# ServiceLocator

Service Locator 패턴은 ServiceLocator 객체에 의존 해소 대상이 되는 객체를 미리 등록해 둔 다음, 인스턴스가 필요한 곳에서 ServiceLocator 를 통해 인스턴스를 받아 사용하는 패턴이다.

- ServiceLocator 패턴이 적용된 예

```java
public class Service {
  
  private final Repository repository;
  
  public Service() {
    // ServiceLocator 를 통해 필요한 인스턴스를 받음
    this.repository = ServiceLocator.Resolve<Repository>();
  }

}
```

이 요청을 통해 반환되는 실제 인스턴스는 시작 스크립트 등을 통해 미리 등록된 인스턴스이다.

- ServiceLocator 에 의존 해소를 위한 정보 미리 등록하기

```java
ServiceLocator.Register<Repository, InMemoryRepository>();
```

- 운영용 Repository 로 교체하기

```java
ServiceLocator.Register<Repository, DataBaseRepository>();
```

이렇게 ServiceLocator 패턴을 사용하면, ServiceLocator에 의존 관계 해소를 위한 정보를 등록하는 부분 혹은 의존 관계가 설정된 시작 스크립트 부분만 수정하면 간단하게 리포지토리를 교체할 수 있다.

ServiceLocator 를 안티패턴으로 보는 사람도 있다. 이유는 다음과 같다.

- 의존 관계를 외부에서 보기 어렵다.
- 테스트 유지가 어렵다.

### 의존 관계를 외부에서 보기 어렵다.

```java
public class Service {
  
  private final Repository repository;
  
  public Service() {
    this.repository = ServiceLocator.Resolve<Repository>();
  }

}
```

위 예제를 미루어보았을때, 의존 관계 해소를 위한 설정이 미리 되어있지 않으면 에러가 발생한다. 

즉, __클래스 정의만 보고 'Service 가 제대로 동작하기 위해서는 Repository 를 사용할 때 전달할 객체를 미리 등록해야 한다'__ 라는 것을 알 수 없다. 

### 테스트 유지가 어렵다.

예를 들어, 새로운 UserRepository 가 추가되었다고 하면

```java
private final Repository repository;
private final UserRepository userRepository;
```

새롭게 추가된 리포지토리에 대한 의존 관계 해소 정보가 등록되어있지 않으면 버그가 발생한다. 문제는 실제로 테스트를 실행하기 전 까지는 알 수 없다는 점이다.

__즉, 컴파일 시점에 알 수 없고, 런타임 시점에 에러 여부를 알 수 있다.__

## References

> [도메인 주도 설계 철저 입문](#)
