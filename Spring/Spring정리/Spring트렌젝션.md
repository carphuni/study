# Spring트렌젝션
트렌젝션을 처리하는 클래스를 제공 : TransactionManager
commit, rollback처리를 담당함.
-> 기본적으로 session클래스가 종료되면 commit함.
-> RuntimeException이 발생되면 rollback처리함.

트렌젝션매니저에 의해 관리된 메소드를 지정(설정)해야지 적용이 됨.

---

## 트렌젝션 옵션
    1. propagation(전파) : 트렌젝션 생성, 기존 트렌젝션에 참여하는 방법에 대한 설정
    REQUIRED : 이미 시작된 트렌젝션이 있으면 참여하고 없으면 새로 생성해서 활용
    SUPPORTS : 이미 시작된 트렌젝션이 있으면 참여, 없으면 트렌젝션을 생성하지 않음
    MANADATORY : 이미 시작된 트렌젝션이 있으면 참여, 없으면 예외처리
    NOT_SUPPORTED : 트렌젠션 없이 진행, 이미 있으면 보류함.
    REQUIRED_NEW : 항상 새로운 트렌젝션을 생성
    NEVER : 트렌젝션을 생성하지 않음

    2. __isolation(격리수준)__ : 트렌젝션에서 수정내용을 다른 트렌젝션에서 사용여부 설정
    READ_COMMITED : 다른 트렌젝션이 커밋하지 않은 정보를 읽을 수 없개 설정
    READ_UNCOMMITED : 커밋되지 않은 데이터를 다른 트렌젝션에서 읽을 수 있게 설정
    REPEATTABLE_READ : 한개의 로우를 트렌젝션에서 읽고있으며 다른 트렌젝션에서 읽을 수 없게 설정
    SERIALZABLE : 가장 강력. 트렌젝션을 순차적으로 실행. 테이블에 접근 차단

    3.timeout : 트렌젝션이 DB에 접속하는 제한시간
    4. read-only, readonly : 읽기 전용
    5. rollback-for, rollbackfor : 트렌젝션에서 rollback하는 기준 Exception 설정
    6. no-rollback-for : rollback예외처리

---

## 설정방법
### 1.xml설정방식
### root-context.xml
    1.1. <tx:config>태그를 이용해서 설정(트렌젝션관련 내용설정)
    
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="insert*"/> name="메소드 명"
		</tx:attributes>
	</tx:advice>
    

    1.2. <aop:config>태그로 메소드 지정

    <aop:config>
		<aop:pointcut expression="execution(* com.bs.spring.board.model..*(..))" id="transPoint"/>
		<aop:advisor advice-ref="txAdvice" pointcut-ref="transPoint"/>
	</aop:config>
    
    
    
### 2. 어노테이션 설정 방식
### root-context.xml
    2.1. root-context.xml에 트렌젝션 매니저 등록하기

	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>	
	</bean>

    2.2.xml 밑 탭namespaces에 tx 체크
    <beans xmlns:tx="http://www.springframework.org/schema/tx"> 추가가 되야함

    2.3.어노테이션으로 트렌젝션을 선언한 내용 가져올수 있게 설정

	<tx:annotation-driven transaction-manager="transactionManager"/>

    2.4.aop-cotext.xml 에 <aop:aspectj-autoproxy/> 작성
### Class   
    2.5. Service메소드에 @Tranactional 어노테이션 추가