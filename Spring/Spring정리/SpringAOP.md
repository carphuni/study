# AOP
## AOP란
메소드..
### 1. 어노테이션 방식
### class
    1.1. 클래스에 @Component로 Bean에 올리기
    1.2. @Aspect 어노테이션 작성
    1.3. Pointcut등록하기 

        ex) @Pointcut("execution(* com.bs.spring.member..*(..))
        public void pointcutMethodName(){}

    1.4. Advisor 등록 (Before, After, Around)

        ex) @Before("pointcutMethodName")
        public void advisorMethodName(JoinPoint jp){
             logic 
        }

        ex)@Around("pointcutMethodName()")
	    public Object demoLoggerAround(ProceedingJoinPoint join) throws Throwable{
		StopWatch stopwatch=new StopWatch();
		stopwatch.start();
		Object obj=join.proceed();//실행 전, 후 기준
		stopwatch.stop();
		log.debug("실행시간 : "+stop.getTotalTimeMillis()+"ms");
		
		return obj;
### aop-context.xml
    1.5.webapp/WEB-INF/spring/appServlet/aop-context.xml생성
    1.6.aop-context.xml에 <aop:aspectj-autoproxy/>작성하여 어노테이션 방식의 등록된 aspect를 불러옴
### web.xml
    1.7.web.xml에 다음과 같이 추가
        <servlet>
            <init-param>
                <param-value>
                    /WEB-INF/spring/appServlet/aop-context.xml
                </param-value>
            </initparam>
        </servlet>

### 2.XML방식
### aop-context.xml
    2.1. webapp/WEB-INF/spring/appServlet/aop-context.xml생성
    2.2. 