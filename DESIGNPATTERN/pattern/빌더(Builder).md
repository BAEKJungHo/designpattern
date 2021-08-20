# 빌더(Builder)

## Example

- 롬복(@Builder) 사용

```java
public class User {
  private String name;
  private String age;
  private String gender;
  
  @Builder
  public User(String name, String age, String gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
  }
}
```

롬복 @Builder 가 적용된 클래스를 컴파일한 파일을 보면 다음과 같이 되어있다.

```java
public class User {
  private String name;
  private String age;
  private String gender;
  
  public static class UserBuilder {
      private String name;
      private String age;
      private String gender;
      
      UserBuilder() {}
      
      public User.UserBuilder name(String name) {
        this.name = name;
        return this;
      }
      
      public User.UserBuilder age(String age) {
        this.age = age;
        return this;
      }
      
      public User.UserBuilder gender(String gender) {
        this.gender = gender;
        return this;
      }
      
      public User build() {
        return new User(this.name, this.age, this.gender);
      } 
      
      public String toString() {
        return "User.UserBuilder(name=" + this.name + ", age=" + this.age + ", gender=" + this.gender + ")";
      }
  }
}
```

- DTO

```java
public class UserDto {

  // 생략
  
  public User toEntity() {
    return User.builder()
      .name(this.name)
      .age(this.age)
      .gender(this.gender)
      .build();
  }

}
```
