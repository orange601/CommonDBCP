# Commons DBCP 이해하기
출처:https://d2.naver.com/helloworld/5102792

# 버전 선택
- JDK(Java development kit)의 버전에 따라서 Connection이나 Statement 같은 
JDBC(Java database connectivity)의 인터페이스가 조금씩 다르므로 사용하는 JDK의 버전에 맞게 
Commons DBCP 버전을 선택해야 안정된 동작을 기대할 수 있다. 
- 예를 들어 JDK 6을 사용하면서 Commons DBCP 1.3.x를 사용한다면 JDBC 4에 추가된 SQLTimeoutException 등을 
드라이버에서 전달할 때 Commons DBCP가 정교하게 처리하지 못해 오류 파악하기 어려울 수 있다.

- Commons DBCP 주요 버전과 그에 대응하는 JDK 버전과 JDBC 버전은 다음과 같다.
  >
  > | Commons DBCP 버전 | JDK 버전 | JDBC 버전 |
  > |:--------|:--------|:--------|
  > | Commons DBCP 2 | 	JDK 7 | 	JDBC 4.1 |
  > | Commons DBCP 1.4 | JDK 6 | JDBC 4 |
  > | Commons DBCP 1.3 | JDK 1.4~1.5 | JDBC 3 |

# 속성 설정
- Commons DBCP 2에서는 패키지 이름이 변경되었다.
  - org.apache.commons.dbcp ▶ org.apache.commons.dbcp2

- Commons DBCP 2.x에서 이름이 바뀐 Commons DBCP 1.x의 속성
  >
  > | Commons DBCP 1.x | Commons DBCP 2.x |
  > |:--------|:--------|
  > | maxActive | maxTotal |
  > | maxWait | maxWaitMills |

# maxWait과 maxActive
- maxWait 속성을 적절하게 설정하지 않아도 일반적인 상황에서는 큰 문제가 되지 않는다. 
  하지만 사용자가 갑자기 급증하거나 DBMS에 장애가 발생했을 때 장애를 더욱 크게 확산시킬 수 있어 주의해야 한다.
- 대기 시간(wait) 값 조절이 무한히 커넥션 개수를 늘리지 않고 최적의 시스템 환경을 구축하는 데 중요한 역할을 한다. 
  이 부분을 이해하려면 Commons DBCP 외에 Tomcat의 동작 방식도 고려해야 한다. 
- 만약 갑작스럽게 사용자가 증가해 maxWait 값 안에 커넥션을 얻지 못하는 빈도가 늘어난다면 maxWait 값을 더 줄여서 시스템에서 
  사용하는 스레드가 한도에 도달하지 않도록 방어할 수 있다. 전체 시스템 장해는 피하고 '간헐적 오류'가 발생하는 정도로 장애의 
  영향을 줄이는 것이다. 이런 상황이 자주 있다면 Commons DBCP의 maxActive 값과 Tomcat의 maxThread 값을 동시에 늘이는 것을 
  고려한다. 그러나 시스템 자원의 한도를 많이 넘는 요청이 있다면 설정을 어떻게 변해도 장애를 피할 수 없다. 
  애플리케이션 서버의 자원이 설정 변경을 수용할 만큼 충분하지 않다면 시스템을 확충해야 할 것이다.
