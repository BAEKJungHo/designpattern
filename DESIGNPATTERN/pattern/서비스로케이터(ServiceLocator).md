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
