# Annotation의 등장 배경

Annotation의 도입 전에는 XML 기반으로 bean 의존성들을 관리하였다.

XML 기반 설정의 대안으로 나온 Java 기반의 의존성 관리가 바로 Annotation인 것이다.

이는 bean component들을 프로그램적으로 관리할 수 있게 해준다.

다만 그렇다고 모든 설정을 Annotation으로 하는 것보다는, 확정적인 부분만 Annotation 기반 설정을 하고, 시스템 전반에 영향을 주며 변경의 여지가 있는 부분은 XML로 설정하여 독립적으로 가지고 있는 게 낫다고 한다.

Annotation을 사용하면 데이터들에 대한 유효 조건을 쉽게 파악할 수 있으며, 코드가 깔끔해진다.

# Reference

https://www.journaldev.com/16966/spring-annotations#spring-annotations

https://palyoung.tistory.com/72