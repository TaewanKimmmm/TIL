# Custom Annotation을 이용한 예외 처리



@Constraint 어노테이션을 달아주는 클래스 하나 + 검증 로직을 가지고 있는 클래스 하나로 가능함

```java
package wooteco.subway.line;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Constraint(validatedBy = NotEqualValidator.class) // 여기서 검증 로직 가지는 클래스랑 매핑
@Target({ElementType.TYPE}) // 클래스 레벨을 의미한다. 어노테이션이 달린 클래스의 필드를 볼 수 있다.
@Retention(RetentionPolicy.RUNTIME)
public @interface NotEqual {
    String message() default "상행 역과 하행 역은 같을 수 없습니다."; // default message

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {}; // 여기까지는 BoilerPlate 이다

    String upStationId(); // NotEqual 어노테이션 적용할 필드명을 리턴함 --> 추후 Reflection에 사용됨

    String downStationId(); // 두 번째 필드를 리턴함

    @Target({ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @interface List {
        NotEqual[] value();
    }
}
```

![image-20210603175954995](/Users/taewankim/Library/Application Support/typora-user-images/image-20210603175954995.png)



```java
package wooteco.subway.line;

import org.springframework.beans.BeanWrapperImpl;
import wooteco.subway.line.dto.SectionRequest;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

//ConstraintValidator<NotEqual, SectionRequest> : 어노테이션 종류, 어노테이션을 적용시킬 객체의 타입
public class NotEqualValidator implements ConstraintValidator<NotEqual, SectionRequest> {
    private String upStationId;
    private String downStationId;

    @Override
    public void initialize(NotEqual constraintAnnotation) {
        this.upStationId = constraintAnnotation.upStationId();
        this.downStationId = constraintAnnotation.downStationId();
    }

    @Override // 꼭 오버라이딩 해야 함
  // SectionRequest or Object
    public boolean isValid(SectionRequest sectionRequest, ConstraintValidatorContext context) {
      // BeanWrapperImpl : 리플렉션을 위한 것
      Object upStationValue 
          = new BeanWrapperImpl(sectionRequest).getPropertyValue(upStationId);
        
      Object downStationValue = 
          new BeanWrapperImpl(sectionRequest).getPropertyValue(downStationId);
        
      return !upStationValue.equals(downStationValue);
    }
}
```

# 실사용

```java
package wooteco.subway.line.dto;

import wooteco.subway.line.NotEqual;

import javax.validation.constraints.NotNull;
import javax.validation.constraints.Positive;

// SectionRequest의 필드명을 String 타입으로 넣어줌. --> NotEqual의 String upStationId(); 에서 이걸 리턴함 --> 추후 NotEqualValidator에서 BeanWrapperImpl를 이용해 이 필드명을 가지고 Reflection하여 값을 가져와서 검증한다. 
// NotEqual의 메서드명이랑 밑에 upStationId는 같아야 한다.
@NotEqual(upStationId = "upStationId", downStationId = "downStationId")
public class SectionRequest {
    @NotNull
    private Long upStationId;

    @NotNull
    private Long downStationId;

    @NotNull
    @Positive(message = "거리는 양수여야 합니다.")
    private int distance;

    public SectionRequest() {
    }

    public SectionRequest(Long upStationId, Long downStationId, int distance) {
        this.upStationId = upStationId;
        this.downStationId = downStationId;
        this.distance = distance;
    }

    public Long getUpStationId() {
        return upStationId;
    }

    public Long getDownStationId() {
        return downStationId;
    }

    public int getDistance() {
        return distance;
    }
}
```

