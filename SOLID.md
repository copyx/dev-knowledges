# SOLID

Robert C. Martin이 2000년대 초반에 명명한 객체지향 프로그래밍 및 설계의 다섯 가지 기본 원칙. 시간이 지나도 유지 보수와 확장이 쉬운 시스템을 만들고자 할 때 적용 가능.

> 응집도는 강하게, 결합도는 약하게

## [Single Responsibility Principle](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)

> 한 클래스는 하나의 책임만 가지며, 그 책임을 완전히 캡슐화해야한다.

책임 = 변경하려는 이유

### Encapsulation(캡슐화)

> 객체의 속성(Fields)과 행위(Methods)를 하나로 묶고, 실제 구현 내용 일부를 외부에 감추어 은닉한다. - Wikipedia

인터페이스와 구현을 분리해 인터페이스는 공개하고 구현은 숨김. 클래스 사용자에게 모든 것 공개한다면 만든 사람이 원하지 않는 방향으로 클래스를 사용할 수 있으며, 이는 결합도를 강하게 만들어 클래스가 수정되면 클래스 사용자의 코드도 수정해야하는 결과를 초래한다.

## [Open Close Principle](https://ko.wikipedia.org/wiki/%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84_%EC%9B%90%EC%B9%99)

소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.

## [Liskov Substitution Prinsiple](https://ko.wikipedia.org/wiki/%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84_%EC%B9%98%ED%99%98_%EC%9B%90%EC%B9%99)

프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.

## [Interface Segregation Prinsiple](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4_%EB%B6%84%EB%A6%AC_%EC%9B%90%EC%B9%99)

특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.

## [Dependency Inversion Principle](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84_%EC%97%AD%EC%A0%84_%EC%9B%90%EC%B9%99)

프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다.
