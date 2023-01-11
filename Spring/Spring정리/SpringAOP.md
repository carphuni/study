# AOP
## AOP란
메소드..
### 1. 어노테이션 방식
    1. 클래스에 @Component로 Bean에 올리기
    2. @Aspect 어노테이션 작성
    3. Pointcut등록하기 

        ex) @Pointcut("execution(* com.bs.spring.member..*(..))
        public void pointcutMethodName(){}

    4. Advisor 등록 (Before, After, Around)

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

    5.webapp/WEB-INF/spring/appServlet/aop-context.xml생성
    6.aop-context.xml에 <aop:aspectj-autoproxy/>작성하여 어노테이션 방식의 등록된 aspect를 불러옴
    7.web.xml
        <servlet>
            <init-param>
                <param-value>
                    /WEB-INF/spring/appServlet/aop-context.xml
                </param-value>
            </initparam>
        </servlet>
    추가
### XML방식