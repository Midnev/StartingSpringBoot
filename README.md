Monolothic Spring boot & Mysql

1. Spring boot dependency & plugin
	- spring 어플리케이션 구조
		- src/main/java : 자바 패키지들이 들어가는 속
		- src/main/resources : 어플리케이션에서 쓰는 기타 파일들(html, css, js 등..)이 저장되는 곳
		- src/main/resources/application.[properties or yaml] : 어플리케이션의 설정
		- pom.xml : 프로젝트에 관한 기본 설정들이 담겨있다.

	- 스프링 부트사용을 선언하기 (Pom.xml)
	--------------------------------------------------------
		<?xml version="1.0" encoding="UTF-8"?>
		<project xmlns="http://maven.apache.org/POM/4.0.0"
		         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		    <modelVersion>4.0.0</modelVersion>

		    <groupId>spring_test</groupId>
		    <artifactId>springtest</artifactId>
		    <version>1.0-SNAPSHOT</version>

		    <dependencies>
		        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
		        <dependency>
		            <groupId>org.springframework.boot</groupId>
		            <artifactId>spring-boot-starter-web</artifactId>
		            <version>2.1.1.RELEASE</version>
		        </dependency>
	        </dependencies>
			<build>
				<plugins>
					<plugin>
						<groupId>org.springframework.boot</groupId>
						<artifactId>spring-boot-maven-plugin</artifactId>
					</plugin>
				</plugins>
			</build>
		</project>
	--------------------------------------------------------
		- project : 해당 태그 내에는 pom.xml을 사용할 때에 필요한 정보들의 위치를 알려주는 url들(xmlns,xsi)이 있다.
		- dependencies : 이 프로젝트에 필요한 외부 라이브러리 파일들의 이름과, 버전이 들어있다. (파일들은 maven repository에서 가져온다,)
		- build	: 파일을 컴파일 할때의 정보들이 들어있다.
		- plugin : 프로젝트에서 사용할 플러그인을 지정해준다. 없어도 main - run으로 작동은 하지만, maven 명령어를 이용한 실행에는 필요하다.

2. SpringBootApplication & Annotation
	- package 생성 (ex. test/application)
	- Application.java 생성 (ex. test/application/Application.java)
	--------------------------------------------------------
		@SpringBootApplication						
		public class Application {
		    public static void main(String[] args) {	
		        SpringApplication.run(Application.class , args);	
		    }
		}
	--------------------------------------------------------
		- @SpringBootApplication : 스프링의 기능들을 사용하기 위한 선언 
		- SpringApplication.run() : 실행할 스프링 클래스를 지정 
		- Application.class : 지금 @SpringBootApplication을 통해서 스프링 기능들을 사용하게 된 클래스를 지정 
		- args : java 어플리케이션을 명령어로 실행하게 될경우 넘겨 받는 파라미터 값을 다시 전달


3. 폴더 구분 & 자동 인식
	- Spring-boot 어플리케이션의 경우 동일 패키지 내의 모든 java 파일들은 자동으로 인식을하여 사용한다.
	- 동일 패키지가 아닌 경우 추가적인 설정을 해주어야 한다.
	--------------------------------------------------------
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
            <version>2.1.1.RELEASE</version>
        </dependency>
	--------------------------------------------------------
		- <dependencies></dependencies> 태그 사이에 추가하고, 다른 <dependency>...</dependency>는 수정하지 않는다.
			- <dependencies>
				<dependency>...</dependency>
				<dependency>...</dependency>
				...
			  </dependencies>

	--------------------------------------------------------
		@ComponentScan(basePackages = {"test"})
		@SpringBootApplication						
		public class Application {
		    public static void main(String[] args) {	
		        SpringApplication.run(Application.class , args);	
		    }
		}
	--------------------------------------------------------
		- @ComponentScan(basePackages = {"spring"}) : spring이라는 폴더 아래에 있는 모든 파일을 자동으로 사용한다는 표시이다.
		- test.application에서 .은 폴더를 구분하는 단위이다. 
			즉 test 폴더 안에 application이 있으며 test내에 다른 패키지를 생성하면 자동으로 인식이 된다는 뜻이다.

4. Controller Service Repositroy 역할
	- spring boot 에서는 java클래스 역할을 크게 4개로 분류한다.(일반 spring 프레임 워크는 config를 xml로 만들 수 있기 때문에 해당이 안된다.) 

		- Controller : url을 받아서 처리를 하는 부분 현재 view(html)과 model(데이터)를 binding(합치는) 역할을 다룬다. (스프링 MVC 참고)

		- Service : 기타 시스템 적인 처리를 한다. 컨트롤러 프로그램이 더러워지는 것을 방지하기 위해서 주 코드는 여기에 작성을 한다.

		- Repository :	DB(데이터베이스)에서 정보를 가져오는 역할을 가지고 있다. 

		- config : 프레임 워크 설정들을 클래스 형태로 만들기 때문에 따로 구분 하였다.

5. Index.html 메인 페이지의 작성
	- 스프링 부트의 경우 resources폴더 내의 static과 templates 폴더는 자동을 인식이 된다. 
		- static : index, error, login등의 html 파일들은 자동으로 인식이 되며 추가적인 설정 없이 바로 볼 수 있다.
		- templates : 기타 html 파일들이 들어가며, view resolver 라는 추가 설정을 통해서 사용이 가능하다.
	- index.html을 static 폴더를 생성 후 안에 작성을 한다.
	--------------------------------------------------------
		<!DOCTYPE html>
			<html lang="en">
				<head>
				    <title>Getting Started: Serving Web Content</title>
				    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
				</head>
				<body>
					<p>hello world</p>
				</body>
			</html>
	--------------------------------------------------------
	- main에서 실행을 하거나 'mvn spring-boot:run'를 해당 디렉토리에서 커맨드라인으로 실행한다.
		- mvn 명령어 설치하기 : https://maven.apache.org/install.html

	- 'http://localhost:8080/' 형태로 홈페이지를 볼 수 있다.(포트 번호는 어플리케이션을 실행하면 나온다.)

6. Thymeleaf & view resolver & Controller & Url-Mapping
	- ThymeLeaf 기능들
		- 컨트롤러에서 html파일과 바인딩 할때 태그 속성을 추가로 제공해서 코딩을 조금 더 쉽게 한다.
		- html 페이지를 파일 이름으로만 인식할 수 있도록 기본적인 view resolver 기능을 제공한다.

	- dependency 추가
	--------------------------------------------------------
	 	<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.1.1.RELEASE</version>
        </dependency>
	--------------------------------------------------------

	- resources폴더 내에 templates 폴더를 추가로 생성 후 first.html 파일을 만든다.
	--------------------------------------------------------
		<!DOCTYPE html>
				<head>
				    <title> first page </title>
				</head>
				<body>
					<div>
					   <p>first page</p> 
					</div>
				</body>
			</html>
	--------------------------------------------------------

	- java 폴더 내에 test.controller 패키지를 새로 만든 후 FirstController.java 파일을 만든다.
	--------------------------------------------------------
        package test.controller;
    
        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.GetMapping;
    
        @Controller
        public class FirstController {
            @GetMapping("/myfirstpage")
            public String firstpage(){
                return "first";
            }
        }
	--------------------------------------------------------
		- @Controller : 해당 클래스는 컨트롤러임을 알려주고 기능이 있을 경우 추가가 된다.
		- @GetMapping :  url을 요청받았을 때 값을 돌려준다는 의미이며 RequestMapping을 세분화한 것 이다.
			명시된 url에 명시된 방법(Get,post 등)으로 요청을 받았을 경우 아래의 함수를 실행한다.
			- @RequestMapping : 아래 모든 기능을 가지고 있다.
				- @GetMapping 
				- @PostMapping
				- @PutMapping
				- @PatchMapping
				- @DeleteMapping

		- "/myfirstpage" : 해당 url로 요청이 들어왔을 때를 지정한다.
			(ex. 'http://localhost:8080/myfirstpage')

		- return "first" : Thymeleaf에서 제공되는 view resolver는 String 형태로 html 파일을 지정할 수 있다.
			이때 first는 'first.html' 파일을 가르킨다.(html 생략)

7. Thymeleaf data binding
	- 'FirstController'에 새로운 메소드를 작성한다.
	--------------------------------------------------------
		import org.springframework.ui.Model;
		import org.springframework.stereotype.Controller;
		import org.springframework.web.bind.annotation.GetMapping;

		@Controller
		public class FirstController {
			@GetMapping("/myfirstpage")
		    public String firstpage(Model model){
		    	model.addAttribute("str","first page");
		        return "first";
		    }

			@GetMapping("/second")
		    public String second(Model model){
		    	model.addAttribute("str","second page");
		        return "first";
		    }
	    }
	--------------------------------------------------------
		- Model : view로 전달할 모델을 뜻한다.
		- model.addAttribute("str","data") : str이라는 이름으로 오른쪽 데이터를 전달한다.
			(문자, 숫자 객체 관개없이 전달할 수 있다.)
		- return "first" : 하나의 페이지를 여러 메소드에서 사용할 수 있다.

	- thymeleaf는 html 페이지에 모델을 바인딩 할때에는 아래와 추가적인 선언이 필요하다.
	- 'first.html' 에 아래와 같이 추가를 한다.
	--------------------------------------------------------
	<!DOCTYPE html>
		<html xmlns:th="http://www.thymeleaf.org">
			<head>
			    <title> first page </title>
			</head>
			<body>
				<div>
					<p>page</p> 
					<p th:text="${str}"></p> 
				</div>
			</body>
		</html>
	--------------------------------------------------------
		- 'xmlns:th="http://www.thymeleaf.org"' : thymeleaf 기능을 사용할 수 있도록 pom.xml과 같은 방식으로 지정하고, th를 약자로 사용하는 것을 지정한다.

		- 'th:text="${str}"' : 문자 형태로 데이터를 바인딩하는 것을 뜻하며, Controller에서 받아온 데이터 중에 str이라는 이름을 가진 데이터를 지정한다.

8. Service & Autowired 

	- test.service 패키지 생성 후 'FirstService.java'를 인터페이스로 생성 
	--------------------------------------------------------
		package test.service;

		public interface FirstService {
			String getData(String str);
		}
	--------------------------------------------------------
		- 인터페이스는 실제로 구현하지 않고 구현해야하는 메소드를 지정해주는 역할을 한다.
			지정된 메소드가 지정된 형태로 구현이 안되어 있을 경우 오류가 난다.

	- 'FirstServiceImple.java'를 test.service 패키지에 구현을 한다.
	--------------------------------------------------------
		import org.springframework.stereotype.Service;

		@Service
		public class FirstServiceImple implements FirstService{
			public String getData(String str){
				str+=" went through service";
				return str;
			}
		}
	--------------------------------------------------------
		- @Service : 해당 클래스는 Service라고 지정한다.
		- intelliJ에서 alt+insert를 통해서 implement methods를 사용하면 
			구현해야할 메소드들을 생성해준다.

	- 컨트롤러에서 접근하기
	--------------------------------------------------------
		import org.springframework.beans.factory.annotation.Autowired;
		import test.service.Testservice

		@Controller
		public class FirstController {
			@Autowired
			FirstService service;

			@GetMapping("/myfirstpage")
		    public String firstpage(Model model){
		    	String new_str = service.getData("string data");
		    	model.addAttribute("str",new_str);
		        return "first";
		    }

	    }
	--------------------------------------------------------
		- @Autowired : 스프링 실행시 생성된 객체 형태를 찾아서 자동으로 연결 해주는 기능이다.
			- 높은 결합성으로 인해 작은 수정이 있을 경우에 전체적인 수정을 피하기 위해서 사용한다.
		- 'FirstService service' : 인터페이스를 지정함으로써 구현된 메소드 이름이 바뀌어도 끌고 올 수 있다.
			- 중복되는 구현체가 있을 경우 '@Qualifier("name")'과 @Serivce("name") 으로 이름을 주어서 해당 이름에 맞는 구현체를 가져올 수 있다.

9. Mysql & 기본 명령어
	- 설치 : 'https://dev.mysql.com/downloads/' Community edition이면 된다.
		- 생성할 때에 등록한 사용자명과 비밀번호를 잘 기억해야한다.
		- workbench를 갈아두면 편하다.
	- 참고 : https://dev.mysql.com/doc/refman/5.5/en/sql-syntax-data-manipulation.html

	- 예시용 게시판 테이블
	--------------------------------------------------------
		create database test;
		use test;
		create table board(
			board_id int(16) primary key auto_increment,
			title varchar(32) not null,
			content varchar(100)
		);

		insert into board(title,content) values ("제목1","내용1");

	--------------------------------------------------------

	- 게시판 테이블 조작용 실습 명령어들
	--------------------------------------------------------
		select * from board where title="제목1" and board_id=1;
		select board_id,title from board;
		select * from board;

		update board set title="t1" where title="제목1";

		delete from board where board_id=3;

		drop table board;

		rollback;
		commit;
	--------------------------------------------------------

10. Mysql & spring-boot 연동 & Repository & Mybatis(DataBase에서 정보를 List로 추출하여서 화면에 출력)
	- Mybatis : database 연동을 편하게 할 수 있도록 해주는 라이브러리
	- Mysql connector : JDBC 드라이버 - 해당 데이터베이스와 연결을 시킬 수 있도록 하는 라이브러리
	--------------------------------------------------------
		//기타 코드 생략
		<dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.13</version>
        </dependency>
        //기타 코드 생략
	--------------------------------------------------------

	- application.properties 확장자를 .yaml로 바꾼 후 
	--------------------------------------------------------
		spring:
		  datasource:
		    url: "jdbc:mysql://localhost:3306/test"
		    username: "root"
		    password: "1234"
	--------------------------------------------------------
		- yaml 파일은 띄어쓰기로 속성을 구분한다 들어가는 깊이에 주의하자
		- spring > datasource > (url, username, password) 
		- url : db에 접근하기 위한 경로가 필요하다
			- 혹시 에러가 발생할 경우 
			?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
			test 바로 뒤에 추가해보자
		- username : DB에 접근하기 위한 사용자 이름이다.
		- password : DB 사용자 비밀번호이다.

	- test.application 패키지에 DBConfig.java를 생성
	--------------------------------------------------------
		import org.apache.ibatis.session.SqlSessionFactory;
		import org.mybatis.spring.SqlSessionFactoryBean;
		import org.mybatis.spring.SqlSessionTemplate;
		import org.mybatis.spring.annotation.MapperScan;
		import org.springframework.context.ApplicationContext;
		import org.springframework.context.annotation.Bean;
		import org.springframework.stereotype.Component;

		@Component
		@MapperScan(basePackages = {"com.mapper"})
		public class DBConfig {

		    @Bean
		    public SqlSessionFactory sqlSessionFactory(DataSource dataSource, ApplicationContext applicationContext) throws Exception{
		        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
		        sqlSessionFactoryBean.setDataSource(dataSource);
		        return (SqlSessionFactory) sqlSessionFactoryBean.getObject();
		    }
		    @Bean
		    public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory sqlSessionFactory){
		        return new SqlSessionTemplate(sqlSessionFactory);
		    }
		}
	--------------------------------------------------------
		- @Component : 해당 객체는 스프링 컴포넌트라는 어노테이션이다.
		- @MapperScan(basePackages = {"com.mapper"}) : Mapper 파일들을 자동을 스캔하는 코드이다. 
		- @Bean : 원래 스프링의 모든 컴포넌트(컨트롤러등도 포함)는 각각 Bean 등록을 해야지만 사용이 가능하지만, ComponentScan을 통해서 자동으로 추가가 되었다. 하지만, 커스텀파일이나, 값을 추가해야하는 객체는 따로 선언해서 집어넣어주어야 한다.
		- 위 코드는 Mybatis에서 DB 연결과 관련이 있으며, 커스터마이징이 아니면 코드를 그대로 사용하는 경우가 많다.
			- Mybatis 이외의 방법으로 DB접속을 할 경우 사용법을 찾아보아야한다.
		- datasource는 application.yaml에서 자동으로 읽어온 객체이다.

	- VO(Pojo) : 객체지향 프로그래밍에서 정보를 전달할 때에 정보만 담은 객체로 전달하는 방법이다.
		- 주로 변수 추적/접근을 제한을 하기 위해서 Getter/Setter를 사용하고 변수들은 private 처리를 하는 경우가 많다.
		- VO(Value Object) 또는 Pojo(Plain old java object)라고 부른다.

	- test.vo 패키지 생성 후 Board_Vo.java 파일을 만든다.
	--------------------------------------------------------
		package test.vo;

		public class Board_Vo {
		    private int board_id;
		    private String title;
		    private String content;
		    //getter setter 생략( IntelliJ : alt+insert)
	    }
	--------------------------------------------------------

	- Mapper : 함수명과 쿼리문을 연결해놓은 클래스 또는 설정 파일
	- test.mapper 패키지를 생성 후 BoardMapper.java 파일을 생성한다.
	--------------------------------------------------------
		package test.mapper;

		import com.vo.Board_Vo;
		import org.apache.ibatis.annotations.Mapper;
		import org.apache.ibatis.annotations.Param;
		import org.apache.ibatis.annotations.Select;
		import org.springframework.stereotype.Repository;
		import java.util.ArrayList;

		@Repository
		@Mapper
		public interface BoardMapper {

		    @Select("select * from board where board_id=#{id}")
		    Board_Vo getBoardByBId(@Param("id") int board_id);

		    @Select("select * from board where board_id between ${first} and ${last}")
		    ArrayList<Board_Vo> getBoardListByRange(@Param("first")int first,@Param("last")int last );
		    
		}
	--------------------------------------------------------
		- @Repository : 해당 객체는 Spring의 데이터 저장 관련 객체이다.
		- @Mapper : 해당 객체는 Mybatis의 mapper 객체이다.
		- interface : mapper 인터페이스의 내용은 자동으로 구현되기 때문에 작성할 필요가 없다.
		- @Select : db에서 select 구문을 사용할때 가져오는 값이 있기 때문에 알려줄 필요가 있다.
		- Board_Vo getBoardByBId() : 조회된 값은 자동으로 Board_Vo 객체에 연결해서 돌려준다.
		- @Param("이름") int board_id : 함수를 호출 할때 받은 변수는 @Param('이름') 으로 위 어노테이션의 쿼리문에서 사용할 수 있다. 오른쪽 변수명과는 무관하다.
		- "select * from board where board_id=#{id}" : Param에서 지정된 이름으로 사용할 수 있다.
		- ArrayList는 조회되는 값이 복수가 조회될 수 있을 때에 각각의 조회 결과를 객체로 담은 뒤 ArrayList로 돌려준다.

	- FirstService에서 접근하기
	--------------------------------------------------------
		//기타 코드 생략
		@Autowired
	    BoardMapper mapper;

	    public Board_Vo onBoardData(int board_id) {
	        Board_Vo vo = mapper.getBoardByBId(board_id);
	        vo.setTitle("[책입니다]"+vo.getTitle());
	        return vo;
	    }

	    public ArrayList<Board_Vo> getBoardByPage(int page, int page_unit) {
	        int start = (page-1)*page_unit+1;
	        int last = (page-1)*page_unit+1+page_unit;
	        return mapper.getBoardListByRange(start,last);
	    }
	    //기타 코드 생략
	--------------------------------------------------------
		- 반드시 해당 메소드 정보를 인터페이스에 선언을 해야지 오류가 나지 않는다.
		- Service를 컨트롤러에 선언하는 것 처럼 선언하고 사용하면 된다.
		- Service는 중간에서 값을 처리하기 위한 객체임을 보여주기 위한 잡 코드를 넣었다.
		- 페이징 처리하는 코드를 추가하였다.

	- Controller에서 접근하기
	--------------------------------------------------------
	@GetMapping("/board")
    public String board(){
        model.addAttribute("boardlist",service.getBoardByPage(page,1));
        return "board";
    }
	--------------------------------------------------------

	- board.html
	--------------------------------------------------------
        <!DOCTYPE html>
        <html xmlns:th="http://www.thymeleaf.org">
        <head>
            <title>board page</title>
        </head>
        <body>
            <table >
                <tr th:each="board : ${boardlist}">
                    <td th:text="'제목입니다 : '+ ${board.title}"></td>
                    <td th:text="${board.content}"></td>
                </tr>
            </table>
        </body>
        </html>
	--------------------------------------------------------
		- th:each : Board 배열의 값이 없을때까지 하나씩 확인하는 for문이다. 반복할때 board 변수에 값을 넘겨준다.
		- board : boardlist에서 넘겨받은 그 1회 반복중인 객체 정보가 들어있다.
		- board.title : 정보는 객체 내에서 추가 정보가 있는 구조이기 때문에 추가적으로 정보를 꺼내오는 방법이다.
		- "'제목입니다 : '+ ${board.title}" : 바인딩 직전에 변화를 줄 수 있다.

11. AOP(Aspect Oriented Programming)
	- 관점지향 프로그래밍이라고도 부르며, 특정 코드의 실행 지점에 코드를 추가할 필요 없이 특정 행동을 할 수 있도록 하는 방법이다.
	--------------------------------------------------------
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
            <version>2.1.1.RELEASE</version>
        </dependency>
	--------------------------------------------------------

	-Application.java에 어노테이션 추가
	--------------------------------------------------------
		import org.springframework.context.annotation.EnableAspectJAutoProxy;

		@EnableAspectJAutoProxy
		@SpringBootApplication
		@ComponentScan(basePackages = {"com"})
		public class Application {
		    public static void main(String[] args) {
		        SpringApplication.run(Application.class , args);
		    }
		}
	--------------------------------------------------------

	- test.application 패키지 내에 AOPConfig.java 생성
	- 사용법은 복잡하기 때문에 간단한 사용법만 적어두었다.
	--------------------------------------------------------
		package test.spring_test;

		import org.aspectj.lang.JoinPoint;
		import org.aspectj.lang.annotation.*;
		import org.springframework.stereotype.Component;

		@Component
		@Aspect
		public class AopConfig {

		    @Before("execution(* test.controller.*.*(..))")
		    public void onBefore(JoinPoint jp){
		        System.out.println("aop log before");
		    }

		    @After("execution(* test.controller.*.*(..))")
		    public void onAfter(JoinPoint jp){
		        System.out.println("aop log after");
		    }

		    @AfterReturning("execution(* test.controller.*.*(..))")
		    public void afterEx(JoinPoint jp){
		        System.out.println("aop log afterret");
		    }
		}

	--------------------------------------------------------
		- @Aspect를 사용한다는 선언
		- execution(* ) : 모든 실행에 대하여
		- test.controller.*.*(..) : test 내의 controller 내의 모든 클래스'*'에 대해서 모든 메소드'*' 중에서 받는 변수 형태의 과계 없이 '(..)' 
				지정시 :  test.controller.FirstController.firstpage()
		- @Before : 메고드 실행 전
		- @AfterReturning : 메소드 성공적인 실행 후
		- @AfterThrowing : 메소드 실행중 오류가 발생시
		- @After : 실패여부 관계 없이 메소드 실행 후 
		- @Around : 실행 중

12. Spring-Boot Security
	- 로그인, 접근제한, 권한부여 관련된 설정
	--------------------------------------------------------
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
            <version>2.1.2.RELEASE</version>
        </dependency>
	--------------------------------------------------------

	- 유저 테이블 작성
	--------------------------------------------------------
        use test;
        create table users(
            user_num mediumint primary key auto_increment,
            user_id VARCHAR(32) NOT null unique,
            user_password VARCHAR(64) NOT NULL,
            user_name VARCHAR(12),
            user_role varchar(12)
        );
        insert into users (user_id,user_password,user_name,user_role)
        values("admin1","1234","name1","ADMIN");
        update users set user_password = "$2a$10$f9D.LbyPmZ5dPSzXmOaV4.zYERdxMzMSfIU7gPaWiSG35ebX73yly" where user_id = "admin1";
	--------------------------------------------------------
		- 패스워드는 암호화되어서 저장하기 때문에 1234이 암호된 값을 따로 빼 두었다.

	- test.vo에 User_Vo.java를 생성 
		- DB에서 유저 정보를 가져오기 위한 객체
	--------------------------------------------------------
		public class User_Vo {

		    int user_num;
		    String user_id,
		            user_password,
		            user_name,
		            user_role;

		    public User_Vo(int user_num, String user_id, String user_password, String user_name, String user_role) {
		        this.user_num = user_num;
		        this.user_id = user_id;
		        this.user_password = user_password;
		        this.user_name = user_name;
		        this.user_role = user_role;
		    }
		    //getter setter 생략
	    }
	--------------------------------------------------------

	- test.vo에 LoginUser.java를 생성
	    - spring security에서 사용하기 위해서 UserDetails을 구현한 객체 
	
	--------------------------------------------------------
		package test.vo;
		import org.springframework.security.core.GrantedAuthority;
		import org.springframework.security.core.userdetails.UserDetails;
		import java.util.Collection;
		public class LoginUser implements UserDetails{
		    int user_num;
		    String user_id,
		            user_password,
		            user_name,
		            user_role;
		    public LoginUser(User_Vo user) {
		        this.user_num = user.user_num;
		        this.user_id= user.user_id;
		        this.user_password = user.user_password;
		        this.user_role="ADMIN";
		    }
		    public Collection<? extends GrantedAuthority> getAuthorities() {return null;}// 접근 권한 관련 설정

		    public String getPassword() {return this.user_password;}

		    public String getUsername() {return this.user_id;}

		    public boolean isAccountNonExpired() {return true;} // 계정 만료 여부 false시 로그인 안됨

		    public boolean isAccountNonLocked() {return true; } // 계정 잠김 여부 false시 로그인 안됨

		    public boolean isCredentialsNonExpired() {return true;} //계정 인증 만료 여부 false시 로그인 안됨

		    public boolean isEnabled() {return true;} // 계정 활성화 여부 false시 로그인 안됨
		}
	--------------------------------------------------------
	    - UserDetails라는 spring의 객체를 구현한 객체이기 때문에 아래 기타 설정들이 존재한다.(implement methods)
		- 주로 User_Vo에서 값이 넘어 올때, 정보처리를 통해서 계정의 상태를 정하는 알고리즘을 넣는게 편하다.
		- 이 객체만 사용할 경우 DB column명이나 반환 관련해서 처리를 해주면 된다. 

	- test.mapper에서 UserMapper.java(인터페이스)를 생성
	--------------------------------------------------------
		@Mapper
		@Repository
		public interface UserMapper {
		    @Select("select *from users where user_id = #{id}" )
		    User_Vo findUserByID(@Param("id")String user_id);
		}
	--------------------------------------------------------
		- 유저 id로 해당 유저 하나만 검색하는 코드이다.
			- @Select : 정보 검색 쿼리
			- @Insert : 정보 입력 쿼리
			- @Update : 정보 수정 쿼리
			- @Delete : 정보 삭제 쿼리

	- UserServiceImple
	--------------------------------------------------------
		import org.springframework.security.core.userdetails.UserDetails;
		import org.springframework.security.core.userdetails.UserDetailsService;
		import org.springframework.security.core.userdetails.UsernameNotFoundException;

		@Service("userservice")
		public class UserServiceImple implements UserDetailsService {

		    @Autowired
		    UserMapper mapper;

		    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
		        User_Vo userdata = mapper.findUserByID(s);

		        if (userdata ==null){
		            throw new UsernameNotFoundException("user not found");
		        }
		        return new LoginUser(userdata);
		    }
		}
	--------------------------------------------------------
		- @Service('userservice') : 해당 객체는 UserDetailsService의 기본 구현체가 존재해서 충돌이 나기 때문에 이름을 준다.
		- UserDetailsService : 스프링 기본 제공 인터페이스로 유저 정보를 관리할 수 있는 서비스를 제공한다.

	- test.application 패키지에 SecurityConfig.java 생성
	--------------------------------------------------------
		import org.springframework.beans.factory.annotation.Autowired;
		import org.springframework.beans.factory.annotation.Qualifier;
		import org.springframework.context.annotation.Bean;
		import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
		import org.springframework.security.config.annotation.web.builders.HttpSecurity;
		import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
		import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
		import org.springframework.security.core.userdetails.UserDetailsService;
		import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
		import org.springframework.security.crypto.password.PasswordEncoder;
		import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

		@EnableWebSecurity
		public class SecurityConfig extends WebSecurityConfigurerAdapter {

		    @Qualifier("userservice") // 해당 이름과 형태에 맞는 bean을 찾아서 온다.
		    @Autowired
		    UserDetailsService service;

		    @Bean
		    public PasswordEncoder passwordEncoder(){return new BCryptPasswordEncoder();} // 패스워드 암호화 객체

		    @Bean
		    public DaoAuthenticationProvider authenticationProvider(){
		        DaoAuthenticationProvider ap = new DaoAuthenticationProvider();
		        ap.setUserDetailsService(service);
		        ap.setPasswordEncoder(passwordEncoder());
		        return ap;
		    } // UserDetailsService를 등록한다.

		    @Override
		    protected void configure(HttpSecurity http) throws Exception {
		        http.authorizeRequests()
		                .anyRequest().authenticated()	// 모든 권한은 인증이되어야한다.
		            .and()
		                .formLogin()					//로그인 설정
		                .loginPage("/login")			//로그인 페이지 설정
		                .loginProcessingUrl("/login")	//로그인 처리 url
		                .failureUrl("/login")//로그인 실패시 이동 페이지
		                .usernameParameter("username")	//id를 담은 파라미터 이름
		                .passwordParameter("password")	//패스워들르 담은 파라미터 이름
		                .permitAll()					// 모두 허가한다.
		            .and()
		                .logout() // 로그아웃 시
		                .deleteCookies("remove")		// 쿠키삭제 설정
		                .invalidateHttpSession(true)	// 세션삭제 설정
		                .logoutRequestMatcher(new AntPathRequestMatcher("/logout")) //로그아웃 처리 url
		                .logoutSuccessUrl("/") 			//로그아웃 성공시 이동 경로 
		            .and()
		                .csrf().disable();				/ajax 사용을 위한 크로스 오리진 필터 비 활성화
		    }
		}
	--------------------------------------------------------

	- controller에 메소드 추가 
	--------------------------------------------------------
		@GetMapping("/login")
	    public String login(){
	        return "/login";
	    }
	--------------------------------------------------------

	- templates에 login.html 추가
	--------------------------------------------------------
		<!DOCTYPE html>
			<html lang="en">
			<head>
			    <meta charset="UTF-8">
			    <title>Title</title>
			</head>
			<body>
			<div class="container">
			    <form class="form-signin" method="POST" action="/login">
			        <h2 class="form-signin-heading">Please sign in</h2>
			        <label for="" class="sr-only">ID</label>
			        <input type="text" name="username" class="form-control" placeholder="ID" required autofocus>
			        <label for=""  class="sr-only">Password</label>
			        <input type="password" name="password" class="form-control" placeholder="Password" required>
			        <div class="checkbox">
			            <label>
			                <input type="checkbox" value="remember-me"> Remember me
			            </label>
			        </div>
			        <button class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
			    </form>

			</div>
			</body>
			</html>
	--------------------------------------------------------






