# [백엔드 스터디 in 큐시즘 - D조] Week 1: 스프링의 전반적인 이해
## #1. 스프링이란 무엇인가

## #2. 스프링과 스프링 부트의 차이는 무엇인가

## #3. 프레임워크와 라이브러리의 차이는 무엇인가

## #4. 스프링 프레임워크의 특징은 무엇인가 - 유빈
스프링의 특징으로 자주 언급되는 내용들은 아래와 같다.

### 1-1. 제어의 역전(IOC)
Spring은 IOC 기반의 프레임워크이다. IOC는 `Inversion of Control`의 약자로, 제어의 역전을 말한다. 이런 역전 패턴은 `프로그램의 생명주기에 대한 제어권`이 `웹 애플리케이션 컨테이너`에 있다. 기존의 모든 작업을 사용자가 제어하던 (각 객체들이 프로그램의 흐름을 결정하고 각 객체를 직접 생성 및 조작 - 이는 객체를 직접 생성하여 호출하는 방식이다.) 방식과 다르게, `사용자의 제어권이 다른 주체에게 넘어가는 것`이라고 할 수 있다. 
이를 java식으로 이야기해보면 코드 작성시 객체의 생성, 의존관계 설정들을 프레임워크가 대신한다는 의미이다. 그러나 이러한 IOC는 직관적이지 못하다는 특징을 지녔고, 이를 DI를 통해 구현할 수 있다. 

이로 인해 아래와 같은 장점을 가질 수 있게 된다.
- 객체의 의존성을 역전시켜 객체 간의 결합도를 줄임
- 유연한 코드를 작성할 수 있게 하여 가독성 및 코드 중복, 유지 보수를 편하게 할 수 있음. 

원래 객체는 다음과 같은 순서로 만들어지지만
`	
1. 객체 생성
2. 의존성 객체 생성 (클래스 내부에서 생성)
3. 의존성 객체 메소드 호출
`

Spring에서는 IoC를 통해 아래와 같은 순서로 생성된다. 
`
1. 객체 생성
2. 의존성 객체 주입 (스스로가 만드는것이 아니라 제어권을 스프링에게 위임하여 스프링이 만들어놓은 객체를 주입한다.)
3. 의존성 객체 메소드 호출

`
위 내용으로 보아, Spring이 모든 의존성 객체를 스프링이 실행될때 다 만들어주고 필요한곳에 주입시켜줌으로써 *Bean들은 싱글턴 패턴의 특징을 가지며, 그 자체가 구조를 설계 할 수 있도록 만들어졌음을 알 수 있다. (하지만, 대부분 프레임워크에서 IoC를 적용하기 때문에 스프링을 IoC 컨테이너라고만 해서는 스프링을 정확히 정의할 수 없다.)

** Bean
Bean은 Spring Bean Container에 존재하는 객체를 말한다. Bean Container는 의존성 주입을 통해 Bean 객체를 사용할 수 있도록 해준다. Bean은 보통 싱글턴으로 존재한다. Beans는 우리가 컨테이너에 공급하는 설정 메타 데이터(XML 파일)에 의해 생성된다.

### 1-2. 의존성 주입 (DI)
DI는 Dependency Injection으로 의존성 주입(객체가 서로 의존하는 관계가 되게 의존성을 주입하는 것)을 의미한다. 외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴인 DI를 통해 객체를 `외부에서 생성한 후 주입` 시키는 방식으로 `모듈간의 결합도를 낮추고` `유연성을 확보`할 수 있다.

예를 들어, 사람이라는 객체와 폰라는 객체가 있을때 사람이 직접 폰을 생성할 경우, 의존성이 높아지게 되고 이는 변경사항이 있을시 영향이 많이 가는 문제를 야기한다. 이때 Spring과 같은 외부에서 직접 생성하여 관리함으로서 두 객체의 의존성이 줄어들게 된다.
``` java
public class People {

    private Phone phone;

    public People() {
        this.phone = new Phone();
    }

}
```

``` java
public class BeanFactory {

    public void people() {
        // Bean의 생성
        Product phone = new Phone();
    
        // 의존성 주입
        Store people = new People(phone);
    }
    
}
```
의존성 주입에는 생성자 주입, 필드 주입, 수정자 주입 등 다양한 주입 방법이 있다.

### 3. AOP
AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 대부분 소프트웨어 개발 프로세스에서 사용하는 방법은 OOP(Object Oriented Programming) 이다. OOP는 객체지향 원칙에 따라 관심사가 같은 데이터를 한곳에 모아 분리하고 낮은 결합도를 갖게하여 독립적이고 유연한 모듈로 캡슐화를 하는 것을 일컫는다. 하지만 이러한 과정 중 중복된 코드들이 많아지고 가독성, 확장성, 유지보수성을 떨어 뜨린다. 이러한 문제를 보완하기 위해 나온 것이 AOP이다.
다시 AOP로 돌아가서, 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다. 여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다. 

- 분별하게 중복되는 코드를 한 곳에 모아 중복 되는 코드를 제거 할 수 있다
- 공통기능을 한 곳에 보관함으로써 공통 기능 하나의 수정으로 모든 핵심기능들의 공통기능을 수정 할 수 있다. -> 효율적인 유지보수가 가능하며 재활용성이 극대화된다.

참고한 블로그
https://mangkyu.tistory.com/150 [MangKyu's Diary:티스토리]
https://khj93.tistory.com/entry/Spring-Spring-Framework%EB%9E%80-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-%ED%95%B5%EC%8B%AC-%EC%A0%95%EB%A6%AC
https://leveloper.tistory.com/33 [꾸준하게:티스토리]
출처: https://engkimbs.tistory.com/746 [새로비:티스토리]


## #5. 스프링 프레임워크를 배우는 이유는 무엇인가 ( 각자의 생각을 공유 )

## #6. 스프링 프레임워크 외에 다른 프레임워크 ( 노드, 장고 등 ) 와의 차별점이 무엇일까

