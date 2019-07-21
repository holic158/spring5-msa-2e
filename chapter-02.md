# Chapter 2 마이크로서비스 관련 아키텍처 스타일 및 사례

- 서비스지향 아키텍처(Service Oriented Architecture)
- 12 요소 어플리케이션
- 서버리스 컴퓨팅
- 람다 아키텍처
- 리액티브 마이크로 서비스
- 마이크로 서비스 프레임워크

## 서비스지향 아키텍처(Service Oriented Architecture)
![soa_archi](/img/c02/soa.jpg)

어플리케이션들의 기능을 비즈니스적인 의미를 가지는 기능 단위로 묶고, 표준화된 호출 인터페이스를 통해서 서비스라는 소프트웨어 컴포넌트 단위로 재 조합한후, 이 `서비스들을 서로 조합(Orchestration)`하여 업무 기능을 구현한 애플리케이션을 만들어내는 소프트웨어 아키텍쳐
SOA는 "서비스 제공자와 사용자가 합의된 계약(또는 인터페이스)을 통하여 느슨하게 연계(Loosely coupled)된 소통을 지원하는 아키텍처를 만들기 위해 필요한 원칙들의 모음"으로 정의된다.
여기서 말하는 느슨한 연계는 서비스 사용자는 자신이 필요로 하는 서비스의 가치(What)에만 집중하고, 서비스 제공자는 사용자가 필요로 하는 서비스를 어떻게(How) 구현할 것인가에만 집중할 수 있는 관계를 의미한다. 그리고, 서비스 사용자와 제공자는 그들 간의 소통을 정의하는 계약(또는 인터페이스)의 합의를 통해 연계되어진다.
즉, SOA는 애플리케이션에서 비즈니스 로직을 분리하여 하나 또는 여러 개의 소프트웨어 모듈에 포함시킨 후 잘 정의된 공개 인터페이스를 통하여 프로그램적으로 접근할 수 있게 노출시키는 방식의 애플리케이션 소프트웨어 토폴로지(Topology)이다.

### SILO
![silo](/img/c02/soa-silo.png)
> 기업 내의 각 부서들이 서로 단절된 상태로 자신의 부서에 필요한 독자적인 어플리케이션을 개발한 것
> 다른 부서의 애플리케이션에서도 유사한 데이터 구조와 기능이 있음에도 불구하고 정보 관리의 유사성은 배제된 채로 각 부서는 서로 다른 독자적인 어플리케이션을 개발
## SILO TO SOA
![silotosoa](/img/c02/soa-silo-to-soa.png)
> SOA의 목표는 조직 내 분산되어 있는 비즈니스 서비스를 해체하여, 여러 시스템에서 공유할 수 있는 공유 비즈니스 서비스 (shared business service)로 통합하는 것

### SOA의 특징
- 자기 완비적(self-oriented)
- 다양한 서비스의 조합으로 구성
- 사용자에게는 블랙박스처럼 내부가 보이지 않음

> SOA와 마이크로서비스는 비슷한 개념을 공유하고 있음.
> 마이크로서비스는 SOA에서 진화하였고, 실제로 많은 특징이 SOA와 비슷함.

### SOA 에서 MSA로
![soa2msa1](/img/c02/soa2msa1.jpg)
어떤 조직에서는 SOA를 어플리케이션 수준에서 적용하기도 한다.
이런 접근 방식에서는 프로토콜 중개, 병령실행, 오케스트레이션, 서비스 통합 같은 횡단 이슈를 해결하기 위해 아파치 카멜이나 스프링 인티그레이션 같은 경량 통합 프레임워크를 어플리케이션 내부에 포함한다. 결과적으로 `모든 서비스가 하나의 일체형 웹 아카이브`에 패킹되고 이런 조직에서는 마이크로서비스를 SOA의 다음 단계라고 생각한다.

![soa2msa2](/img/c02/soa2msa2.jpg)
일체형 시스템에서 일체형 어플리케이션을 더 작은 단위로 변형시킬 때 SOA가 적용된다. 어플리케이션을 작은 크기의 물리적으로 배포 가능한 하위 시스템으로 분리하고, 웹 아카이브로 만들어 웹서버에 배포하거나 JAR로 만들어 자체 컨테이너에 배포할 수 있다. 서비스 간의 데이터 교환을 위해 웹 서비스나 경량 프로토콜(HTTP, JMS...)이 사용된다. 이 방식에서 SOA와 MSA는 알고보면 옛날 것과 똑같다고 생각한다.

## 12 요소 어플리케이션(The twelve-factor methodology)
The Twelve-Factor App methodology is a methodology for building SaaS applications. These best practices are designed to enable applications to be built with portability and resilience when deployed to the web
> 1. 단일 코드 베이스 원칙(Codebase)<br/>
각 어플리케이션이 하나의 코드 베이스만을 가져야 함. dev, test, prod 동일한 하나의 코드 베이스를 기반으로 구성. git, svn 같은 형상관리 도구를 통해 관리
> 2. 의존성 꾸러미(Dependencies)<br/>
모든 어플리케이션은 필요한 모든 의존성을 어플레키션과 함께 하나의 꾸러미로 담음. Maven-pom.xml, Gradle-.gradle을 통해 의존성 관리
All dependencies should be declared, with no implicit reliance on system tools or libraries.
> 3. 환경설정 외부화(Config)<br/>
모든 환경 설정 파라미터를 코드와 분리해서 외부화.
> 4. 후방 지원 서비스 접근성(Backing Service)<br/>
모든 후방 지원 서비스는 URL을 통해 접근 가능해야 함. 모든 서비스는 실행주기 동안 외부의 자원과 의사소통해야함
> 5. 빌드, 출시, 운영의 격리(Build, release, run)<br/>
빌드, 출시, 운영의 격리 원칙에 따라 단계를 뚜렷하게 격리해야함.
빌드: 모든 자원 컴파일 -> 바이너리 만듦
출시: 바이너리 + 환경설정 파라미터와 혼합
운영: 실행 환경에서 어플리케이션 운영
빌드, 출시, 운영의 일방향 파이프라인을 통과하여 서비스 변경을 적용
> 6. 무상태, 비공유 프로세스(Processes)<br/>
프로세스들이 상태가 없어야 하고, 아무것도 공유하지 않는 것이 좋음. 상태를 저장해야 할 경우 DB나 REDIS를 사용하여 처리
> 7. 서비스를 포트에 바인딩하여 노출(Port Binding)
12 요소 어플리케이션은 Self-Contained 또는 StandAlone이어야 함. 외부 웹서버에 의존하지 않고 HTTP 리스너, 서비스리스너를 애플리케이션 자체에 내장.
> 8. 확장을 위한 동시성(Concurrency)<br/>
MSA는 서비스가 서버의 자원을 늘리는 수직적 확장(Scale up)이 아닌 서버의 수를 늘리는 수평적 확장(Scale out) 박식으로 확장된다. 복제를 통해 프로세스가 확장될 수 있게 설계해야 한다.
> 9. 폐기 영향 최소화(Disposability)<br/>
서버의 startup과 shutdown에 필요한 시간을 최소화하고, 서버가 종료될 때 종료에 필요한 작업이 모두 수행되어야 한다. 완전 자동화를 위해 최소한의 시동/종료 시간을 갖도록 애플리케이션의 크기를 가능한 작게 유지하는 것이 극단적으로 아주 중요하다.
> 10. 개발과 운영의 짝맞춤(Dev/Prod parity)<br/>
개발 환경과 운영환경을 간으한 동일하게 유지. 인프라스트럭처 구성에 더 많은 비용이 들지만, 운영 환경 장애나 장애 해결에 도움이 됨.
> 11. 로그 외부화(Logs)<br/>
로컬 I/O를 피해 병목현상이 발생하지 않도록 중앙 집중식 로깅 프레임워크를 사용하는 것을 추천. MSA에서는 서비스를 쪼개므로, 로그가 분산될 가능성이 있음. 로그를 중앙 집중화 하는 것이 매우 중요.
> 12. 관리자 프로세스 패키징(Admin Processes)<br/>
어플리케이션 본연의 서비스와 별개로 관리자용 태스크가 필요하다. 기능 점검을 위한 일회성 스크립트 실행, 어플리케이션 모델 확인 등등. Any needed admin tasks should be kept in source control and packaged with the application.

## 서버리스 컴퓨팅 아키텍처(Serverless Computing Architecture)
서버리스 컴퓨팅 아키텍처 또는 서비스로서의 기능(Faas, Function As a Service)에서는 개발자가 어플리케이션 서버, 가상머신, 컨테이너, 인프라, 확장성에 대해 고민할 필요가 없다. 개발자는 비즈니스 로직을 담고 있는 함수를 작성해서 현재 실행되고 있는 컴퓨팅 인프라에 떨구기만 하면 된다. 서버리스 컴퓨팅은 NoOps라고 불리기도 한다.
AWS Lambda, IMB OpenWhisk, Azure Functions, Google Cloude Functions이 현재 서버리스 컴퓨팅에서 널리 사용되는 관리형 인프라스트럭처다.

### MSA과 서버리스 컴퓨팅
MSA는 서버리스 컴퓨팅의 기초가 되며 공통된 특징이 많음

- 함수는 한 번에 하나의 임무만 수행하고 외부와 격리되어 있다.
- 이벤트 기반 또는 HTTP 기반의 API를 통해 외부와 통신

![aws](/img/c02/awslambda-archi.jpg)

Login, Area, Ranking 각 마이크로 서비스는 서로 분리된 AWS 람다 함수로 개발되고 HTTP 기반의 API 게이트웨이를 통해 통신한다. 

## 람다 아키텍처
![rambda-def](/img/c02/rambda-def.png)

The LA aims to satisfy the needs for a robust system that is fault-tolerant, both against hardware failures and human mistakes, being able to serve a wide range of workloads and use cases, and in which low-latency reads and updates are required. The resulting system should be linearly scalable, and it should scale out rather than up.

1. All data entering the system is dispatched to both the batch layer and the speed layer for processing.
2. The batch layer has two functions: (i) managing the master dataset (an immutable, append-only set of raw data), and (ii) to pre-compute the batch views.
3. The serving layer indexes the batch views so that they can be queried in low-latency, ad-hoc way.
4. The speed layer compensates for the high latency of updates to the serving layer and deals with recent data only.
5. Any incoming query can be answered by merging results from batch views and real-time views.

![rambda-def](/img/c02/rambda.png)

![rambda-def](/img/c02/rambda2.png)

![rambda-msa](/img/c02/awslambda-msa.jpg)

- MSA는 배치 계층 위에서 데이터를 처리하고 서빙 계층으로 전달한다. MSA는 독립적이므로 새로운 요구사항이 발생하면 해당 기능을 MSA로 구성해서 쉽게 추가할 수 있다.
- 데이터를 외부에 제공하는 서빙 계층에서도 마찬가지로 MSA가 서빙 계층 위에서 API를 통해 데이터를 외부에 전달한다.

## 리엑티브
> 사용자가 해당 소프트웨어를 사용하기 위해서 어떤 입력을 발생시켰을 때 꾸물거리지 않고 최대한 빠른 시간 내에 응답을 한다는 의미
1. 응답성(Responsive)
시스템이 가능한 한 즉각적으로 응답하는 것을 응답성이 있다고 한다. 문제를 신속하게 탐지하고 효과적으로 대처할 수 있는 것을 의미한다. 응답성이 있는 시스템은 신속하고 일관성 있는 응답 시간을 제공하고, 신뢰할 수 있는 상한선을 설정하여 일관된 서비스 품질을 제공한다. 이러한 일관된 동작은 오류 처리를 단순화하고, 일반 사용자에게 신뢰를 조성하고, 새로운 상호 작용을 촉진한다. 
2. 탄력성(Resilient)
시스템이 장애에 직면하더라도 응답성을 유지하는 것을 탄력성이 있다고 한다. 탄력성이 없는 시스템은 장애가 발생하면, 장애가 전파되고 응답성을 잃게 된다.
3. 유연성(Elastic)
시스템이 작업량이 변화하더라도 응답성을 유지하는 것을 유연성이라고 한다. 리액티브 시스템은 입력 속도의 변화에 따라 이러한 입력에 할당된 자원을 증가시키거나 감소시키면서 변화에 대응한다. 시스템에서 경쟁하는 지점이나 중앙 집중적인 병목 현상이 존재하지 않도록 설계하여, 구성 요소를 샤딩하거나 복제하여 입력을 분산시키는 것을 의미한다. 실시간 성능을 측정하는 도구를 제공하여 응답성 있고 예측 가능한 규모 확장 알고리즘을 지원한다. 이 시스템은 하드웨어 상품 및 소프트웨어 플랫폼에 비용 효율이 높은 방식으로 유연성을 제공한다.
4. 메시지 주도(Message Driven)
비동기 프로토콜을 기반으로, 메시지 기반으로 느슨하게 결합한 시스템 아키텍처를 구성한다. 명시적인 메시지 전달은 시스템에 메시지큐를 생성하고, 모니터링하며 필요시 BackPressure 를 적용하여 유연성을 부여한다. 이런 메시지 기반 시스템은 큐 분산 시스템으로 시스템 부하를 막고 안정적인 시스템을 운영할 수 있다.

## 리액티브 프로그래밍

 데이터 흐름과 전달에 관한 프로그래밍 패러다임. 기존의 명령형 프로그래밍은 주로 컴퓨터 하드웨어를 대상으로 프로그래머가 작성한 코드가 정해진 절차에 따라 순서대로 실행되는 방식. 리액티브 프로그래밍은 데이터 흐름을 먼저 정의하고 데이터가 변경되었을 때 연관되는 함수나 수식이 업데이트되는 방식.

 1. 비동기 데이터 스트림을 이용
- 데이터 스트림이 어플리케이션의 뼈대
- 이벤트, 메시지, 호출, 실패를 데이터 스트림으로 전달
- 이 스트림을 Observe(Subscrive)하고, 값을 발생(emit)할 때 반응
- 모든 것이 스트림: 클릭 이벤트, HTTP 요청, 유입 메시지, 변수 변경, 세선 값 등 바뀌거나 발생할 수 있는 모든 것 (이는 본질적으로 비동기와 관련)
2. 콜드(Cold) vs 핫(Hot)
- 콜드 스트림 : 누군가 Observe를 시작할 때까지 아무것도 안 함. 스트림이 소비될 때 동작하기 시작. 여러 subscriber가 공유하지 않음. 예) 파일 다운로드
- 핫 스트림 : 소비 전에 활성. 개별 subscriber에 독립적으로 데이터 생성. 구독하기 시작한 이후의 데이터 수신. 예) 실시간 주식 주가, 센서 데이터
3. 비동기 오용 주의
- 부수 효과 : 불필요한 부수 효과를 피할 것. 부수 효과 없는 함수 -> 쓰레드 안정성
- 쓰레드 : 너무 많은 쓰레드 사용을 피할 것. 다중 쓰레드는 동기화 문제 위험
- 블록 : 쓰레드를 소유하지 않으므로 쓰레드를 절대 블록하지 말 것

## Loosely coupled를 위한 리액티브 마이크로서비스
![reactive-msa](/img/c02/reactive-msa-ex.png)

주문과 결재, 배송서비스들은 서로 공유되어있다. 각 서비스들을 동기 호출하면 서비스 간 강한 의존성을 가지게 되기 때문에 MSA의 강점을 충분하게 살릴 수 없다. 결국 모노토릭 서비스와 크게 다를게 없어진다.

그래서 도입되는 개념이 리액티브 마이크로 서비스이다. 
서로간에 마이크로서비스가 독립적으로 존재하므로 특정 부분에 문제가 발생하면 해당 마이크로서비스의 복제본이 이를 대체할 수 있다. 

하지만 이렇게 격리가 되었다고 해도 서비스 사이의 통신 방식과 의존 관계가 동기 블로킹방식의 RPC로 구성된다면 서비스는 완전히 격리된 것이 아니고 서로 의존관계가 있기에 오류가 전파될 수 밖에 없다.  

그래서 서비스 사이의 통신을 비동기 논블로킹 호출을 사용하는 리액티브 스타일로 설계하는 것이 중요.

각 마이크로서비스들은 이벤트를 리스닝 하고있다. 각 서비스에서 발생한 출력값이 다른 서비스에 입력값으로 들어가게 되는데 특정 마이크로서비스의 값을 대기하는게 아니라 단지 자신의입력 큐에 이벤트가 들어오기를 기다린다. 

마이크로 서비스들은 서로의 존재를 알지 못하고, 단지 이벤트 발생을 리스닝 하고 있다. 이렇게 되면 한 서비스의 결과 큐를 다른 서비스의 입력 큐로 연결하는 연출(choreography)효과를 낼 수 있다.  

## 마이크로서비스 프레임워크
- 스프링부트 + 스프링클라우드
- 드랍위자드
- 와일드플라이 스윔
- 스프링5(리액티브 웹 프레임워크) + 스프링 부트 
- 스프링 클라우드 스트림
- 스파크 : REST 개발 시 사용되는 마이크로 프레임워크
- 애저 서비스 패브릭: MS 사에 서 만든 마이크로서비스 프레임워크

