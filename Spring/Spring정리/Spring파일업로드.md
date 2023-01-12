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

---

## jsp
	* <form>태그에 method="post" enctype="multipart/form-data" 확인!

---

## class
```java
//ex)
@RequestMapping("boardwriteend.do")
//	public ModelAndView boardWriteEnd(MultipartFile upFile, ModelAndView mv, HttpSession session) {//한개
public ModelAndView boardWriteEnd(MultipartFile[] upFile, ModelAndView mv, HttpSession session) {//여러개
	//1. 저장위치 가져오기
	String path=session.getServletContext().getRealPath("/resources/upload/board/");
	//2. 폴더있는지 확인하고 폴더를 생성해주기
	File dir=new File(path);
	if(!dir.exists()) dir.mkdirs();
	
	List<Attachment> files=new ArrayList<Attachment>();
	
	for(MultipartFile f : upFile) {//여러개
		//3. 리네임드 규칙을 생성하기
//		if(!upFile.isEmpty()) {//한개
		if(!f.isEmpty()) {//여러개
			//3.1. 전송된 파일이 있다면 파일 리네임 처리 직접하기
//			String originalFileName=upFile.getOriginalFilename();//한개
			String originalFileName=f.getOriginalFilename();
			String ext=originalFileName.substring(originalFileName.lastIndexOf("."));
			//3.2. 중복되지 않는 이름 설정하는 값 지정하기(리네임 규칙)
			SimpleDateFormat sdf=new SimpleDateFormat("yyyyMMdd_HHmmssSSS");
			int rnd=(int)(Math.random()*10000)+1;
			String renameFile=sdf.format(System.currentTimeMillis())+"_"+rnd+ext;
			
			//3.3. 파일 업로드하기
			try {
				//3.3.1. MultipartFile클래스가 제공해주는 메소드 이용해서 저장처리
//				upFile.transferTo(new File(path+renameFile));//한개
				f.transferTo(new File(path+renameFile));
				files.add(new Attachment().builder()
						.originalFileName(f.getOriginalFilename())
						.renamedFileName(renameFile).build());
			}catch(IOException e) {
				e.printStackTrace();
			}
			
		}
	}
	
	return mv;
}
```
	