# 3주차

## System Call 시스템콜
 운영체제의 서비스를 받기 위해  운영체제에 정의된 함수(커널영역에 있는)를 호출하는 것을 말한다. 
 사용자가 운영체제의 서비스(특권 명령, 즉 커널 함수를 호출하기 위해)를 요청하기 위해 커널의 함수를 호출하는 것.


 -> 소프트웨어 인터럽트의 한 종류이다. 트랩이라근 용어로 부른다. 시스템콜과 예외 상황 2가지로 나뉜다.

일반적으로 함수 호출을 할 경우에는 스택의 복귀주소를 저장한 후 호출한 함수 위치로 점프하지만
시스템콜은 자신이 인터럽트 라인에 인터럽트를 세팅하는 명령을 통해 이루어진다. 

- 시스템콜 처리 과정

  1. 디스크 파일을 읽어와야 할 경우 시스템 콜을 한다. 

  2. 인터럽트 라인에 명령을 세팅한다. 

  3. cpu는 다음 명령을 수행하기전 인터럽트 라인을 체크한다. 

  4. 수행 중인 프로그램을 멈추고 , 상태를 저장하고 , 인터럽트를 발생시킨다. 모드비트 1로 변경

  5. 인터럽트 백터를 통해 인터럽트 처리 루틴을 찾아 이동한다. 

  6. 데이터를 읽어오는 일을 컨트롤러, 로컬버퍼에게 맡기고, cpu 제어권을 다른 프로세스에게 넘긴다. 

  7. 이때 프로세스는 봉쇄상태가 된다. 

  8. 작업이 완료되면 컨트롤러는 인터럽트 라인을 설정한다. 

  9. cpu는 명령어를 수행 하기전에 인터러트 라인을 체크하고 인터럽트가 발생한다. 하던 것을 멈추고 상태 저장

  10. 로컬버퍼에서 메모리로 얻어오고, 봉쇄당한 프로세스를 다시 준비 상태로 만들어 준비 큐에 넣는다.

  11. 다시 인터럽트 당했던 프로세스에게 넘어가 하던 작업을 수행한다.  

  12. 항상 명령어를 수행 하기전 모드비트를 검사한다. 


  ## 프로세스와 스레드 

프로세스는 자원을 할당받는 할당의 단위이고 스레드는 할당 받은 자원을 사용하는 실행의 단위이다. 

프로세스는 실행중인 프로그램으로, 디스크에 파일 형태로 존재하던 프로그램이 cpu로 부터 메모리를 할당받은 상태로 프로그램의 인스턴스라고 볼 수 있다. 


- CPU 디스패치 

준비 큐로부터 cpu를 할당받을 프로세스를 선택 후 실제로 cpu의 제어권을 넘기는 과정이다. 

실행중인 프로세스의 문맥을 PCB에 저장하고 준비큐로부터 새롭게 선택된 프로세스의 문맥을 PCB로 부터 복원한 후 

프로세스에게 cpu를 넘기는 과정



## 웹 데이터의 흐름, 클라이언트 PC로 백엔드까지...


1.  사용자가 웹 브라우저 url 입력 (www.naver.com)
2.  DNS 서버에서 웹서버 호스트 이름을 IP로 변경한다. 
  - 0. host file 검사, 
  -  PC에 설정되어있는 DNS 서버, local DNS에 해당 호스트 네임이 있는지 확인한다. 모르면 
  - local DNS는 Root DNS 13개 에게 물어본다. Root DNS는 Top-level를 확인하여 알려준다.
  - local DNS는 Top-level DNS 에서 호스트 네임을 물어보게 되고, Second-level DNS를 알게 됩니다. Top-level DNS : com DNS
  - Second DNS : naver.com DNS 는 해당 IP를 가지고 있고 알려주게 됩니다. (authoritative DNS)
3. 웹 서버와 tcp 연결
  - sync (Sequence Number에 랜덤한 숫자를 담는다.  Connection을 맺을 때 사용한 포트는 시간이 지나서 재사용하게 되는데, 따라서 과거에 사용했던 포트를 사용할 경우가 존재하기 때문에, 만약 0부터 순차적으로 시작된다면 이전 Connection으로부터 오는 패킷으로 인식할 수 있기 때문이다.)
  - ack
  - sync + ack
4. https SSL 동작 
 - Client Hello (클라이언트가 생성한 랜덤 데이터, 클라이언트가 지원하는 암호화 리스트)
 - Server Hello ( 서버에서 생성한 랜덤 데이터 , 서버가 선택한 클라이언트의 암호화 방식, 인증서)
 - Client 인증서 확인 (서버가 발급한 인증서가 브라우저에 내장된 CA 리스트 검사를 한다. 없으면 경고메시지를 출력)
   - 클라이언트 인증서 공개키로 복호화해서 공개키를 흭득한다.
   - 클라이언트, 서버가 생성한 랜덤 데이터를 공개키로 암호화한다.
 - Server 복호화 (서버는 비공개키로 복호화하고 해당 키를 통해 세션을 생성한다. 세션 키가 결국 대칭키로 사용된다.)
5. LB (7이라 가정)
  - 라운드 로빈 방식으로 서버에 전달
6. 서버 요청(Spring Tomcat, WebMVC 가정)
  - Coyote(http 컴포넌트, tcp를 통한 프로토콜 지원)가 요청을 받아서 Catalina(서블릿 컨테이너)로 넘김
  - Filter 처리
  - DispatcherServlet 요청이 들어옴
  	- HandlerMapping
  	  : annotation requestMappingHandlerMapping
  	  HandlerExecutionChain을 리턴하게 된다. 해당 부분에 인터셉터, Controller method가 껴있다.
  	- HadnelrAdapter 
  	  : annotation RequestMappingHandlerAdapter
  	  해당 처리에서 조금 특이한 것은 getDefaultReturnValueHandlers를 통해 HandlerMethodReturnValueHandler를 따로 처리한다. 여기서 대표적으로 RequestResponseBodyMethodProcessor 친구가 있다. 
  	- ViewResolver
  	  : freemarkerViewResolver
  	  이건.. 뭐 다 알다 싶이...
    - 예외 발생시.. HandlerExceptionResolver 처리.. 이건 나중에
7. 클라이언트 html 문서 도착 
8. 브라우저 동작 원리.. 이부분은 FE에게..
9. https 세션 죵료
10. tcp 종료 4ways hand shake
 - FIN
 - ACK, 종료 프로세스 실행
 - FIN
 - ACK, TIME_WAIT
   TIME_WAIT 문제 : 서버로부터 받지못한 데이터가 있을 것을 대비해서, 연결이 종료되었는지 확인하기 위해서 해야한다. 만약 클라이언트의 마지막 ACK가 유실할 경우 서버는 LAST_ACK상태에 빠지게 되는데 
   새로운 연결을 시도하는 SYN을 시도시, 이미 연결 상태이기 때문에 오류메시지가 출력된다.

  






 









