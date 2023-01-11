# Spring 파일업로드
## Settiing
	1. pom.xml에 파일업로드를 위한 라이브러리 등록(commons.io, commons-fileupload)
	2. webapp/WEB-INF/spring/appServlet/servlet-context.xml에 파일업로드 처리할 resolver를 등록하기

	<beans:bean 
	id="multipartResolver"
	class="org.springframework.web.multipart.commons.CommonsMultipartResolver"
	>
		<beans:property name="maxUploadSize" value="104857600"/> //value="1024*1024*100" 파일 크기=100Mb
	</beans:bean>

## jsp
	1. <form>태그에 method="post" enctype="multipart/form-data" 확인!