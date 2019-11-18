spring boot 시작하기 from scratch(MSA)

환경 추천 
	- intelliJ

1. new project 생성 (spring initializr을 사용할 경우 읽기만하고 5번에서부터 실습 시작)
	-Maven 프로젝트 생성 

2. 프로젝트 구조 

	- src/main/java : 자바 클래스 파일들을 패키지 단위로 분류하는 곳
	- src/main/resources : 설정,자바스크립드,html,css 등의 기타 파일들 
	- ./pom.xml		: 웹 어플리케이션의 dependancy,plugin, 버전 등의 정보

3. dependency 추가 (Maven repository 검색)
		-------------------------------------------------------------------------------------
		<?xml version="1.0" encoding="UTF-8"?>
		<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
			<modelVersion>4.0.0</modelVersion>
			<groupId>com.example</groupId>			
			<artifactId>demo</artifactId>			--->빌드 파일명
			<version>0.0.1-SNAPSHOT</version>		---> 버전 명
			<name>demo</name>
			
			<description>Demo project for Spring Boot</description>			---> 설명 
			<properties>
				<java.version>1.8</java.version>
			</properties>
			<dependencies>			---> 해당 태그는 pom.xml 아래에 하나만 존재할 수 있다.
				<dependency>
					<groupId>org.springframework.boot</groupId>
					<artifactId>spring-boot-starter-test</artifactId>
					<version>2.1.2.RELEASE</version>
				</dependency>
			</dependencies>
			<build>
				<plugins>
					<plugin>
						<groupId>org.springframework.boot</groupId>	--> 스프링 부트 플러그인 사용
						<artifactId>spring-boot-maven-plugin</artifactId>
					</plugin>
				</plugins>
			</build>
		</project>
		-------------------------------------------------------------------------------------

4. 기본 웹 어플리케이션의 제작 

	- src/main/java 디렉토리 아래에 package 생성
	- 생성된 패키지 아래에 Application 이라는 이름의 클래스 파일을 생성
		-------------------------------------------------------------------------------------
			import org.springframework.boot.SpringApplication;
			import org.springframework.boot.autoconfigure.SpringBootApplication;

			@SpringBootApplication		-->스프링 어플리케이션 기본 코드를 사용하는 어노테이션
			public class Application {								
				public static void main(String[] args) {			-->어플리케이션 메인
					SpringApplication.run(Application.class, args); 
						--> 스프링 어플리케이션 어노테이션이 붙은 클래스를 실행하도록 전달해준다.
				}

			}
		-------------------------------------------------------------------------------------

================================================================================
Spring initializr을 사용할 경우 위와같은 내용은 자동적으로 생성이 된다.
================================================================================

5. 어플리케이션 실행하기 
	- mvn 명령어 : 설치하기 https://maven.apache.org/install.html
		- mvn spring-boot:run
	- mvnw 명령어 : initializr을 사용했을 경우 폴더에 해당 파일이 있다.
	  해당 파일은 Maven wrapper라고 부르며, mvn 명령어가 설치되어있지 않아도 어플리케이션을 실행시킬 수 있다.
		- mvnw spring-boot:run 

===============================================================================

6. RESTful 게시판 만들기
	- 필요한 Depnedency : H2(메모리 데이터 베이스), REST Repositories, JPA
		(버전은 생략됨 - initializr의 경우 그냥 아래 내용을 추가한다.
			parent부분에 버전 관련 정보가 잇기 때문에 해당 버전을 가져오는 것이다.)
		-------------------------------------------------------------------------------------
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-rest</artifactId>
		</dependency>

		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		-------------------------------------------------------------------------------------

	- Entity 객체 생성 - 테이블을 자동을 생성해준다. (GETTER, SETTER도 추가해준다. intelliJ에서는 alt + insert)
		-------------------------------------------------------------------------------------
		import javax.persistence.Entity; -->반드시 persistance 사용할 것
		import javax.persistence.GeneratedValue;
		import javax.persistence.Id;

		@Entity 			--> 해당 객체는 데이터 저장용 JPA 객체임을 뜻한다.
		public class Board { 

		    @Id 			--> 어노테이션 아래에 있는 변수는 primary key 이다.
		    @GeneratedValue --> 어노테이션 아래에 있는 변수는 자동으로 값이 증가한다. 
		    Integer board_number; -->어노테이션이 여러개 일 경우에도 변수 위에 있는 것들만 적용된다.

		    String title;
		    String content;
	    }
		-------------------------------------------------------------------------------------
    - Repository Interface 생성 
   		-------------------------------------------------------------------------------------
		import org.springframework.data.repository.PagingAndSortingRepository;

		public interface BoardRepository extends PagingAndSortingRepository<Board,Integer> {
		}
		-------------------------------------------------------------------------------------
		- PagingAndSortingRespostory 인터페이스 : 해당 인터페이스는 어플리리케이션 실행시 자동으로 REST 정보로 통신할 수 있는 코드가 있다.
		- <>안의 내용은 <자동으로 정리할 객체, 객체의 primary key형태> 이며 정리할 객체에 대한 구분을 하기 위해서 사용이 된다.
	
	- 어플리케이션을 실행한다. 올라온 포트번호를 확인한다.

	-실행된 어플리케이션에서 http://localhost:8080/ 형태로 위의 포트번호를 넣어서  접속할 경우 JSON 형태로 정보가 전달되는 것을 볼 수 있다. 

	- 어플리케이션의 사용 (curl을 사용하여도 상관은 없으나, 문장이 복잡해지기 때문에 httpie를 사용할 것을 권장한다.)
		- httpie 설치하기 https://httpie.org/doc
		- httpie 설치후 사용하기 : 'http POST http://localhost:8080/boards title='title1' content='content1'
			- POST - 정보를 추가한다, GET - 정보를 가져온다, PUT - 정보를 수정한다, DELETE - 정보를 삭제한다.  
			- title/content - entity 객체의 파라키터 값   
			- boards - entity 객체 저장공간으로 접속 
			- http 명령어 - httpie 

7. 댓글 추가하기 
	- 댓글 객체 생성 (getter/setter 추가 생성)
		-------------------------------------------------------------------------------------
		@Entity 			
		public class Comment { 

		    @Id 			
		    @GeneratedValue 
		    Integer comment_number;
		    String content;

		    @ManyToOne @JoinColumn(name = "board_number")		--> 어노테이션은 스페이스바로 이어서 적어도 된다
		    Board board;

	    }
	    -------------------------------------------------------------------------------------
		- JPA Associations (객체 관계를 지정)
    		- ManyToOne  - 이 객체 형태(JPA 엔티티)들은 대상 객체(어노테이션이 지정하는 객체)에 여러개가 들어간다.
    		- ManyToMany -	
    		- OneToMany  - 이 객체는 대상 객체를 여러개 가진다.
    		- OneToOne	 - 이 객체는 대상 객체 형태를 하나를 가진다.
    		**여기서 가진다는 테이블 구조로 연결되어있다는 의미이며 실제로 정보가 들어있는 것이 아니라 주소가 들어간다.** 
    	- JoinColumn - 이 테이블은 대상(어노테이션이 지정하는 객체) 어떤 이름으로 호출이 되는지 지정 (부모와 관계를 표현하기 위해서 사용)

	- 게시판 객체에서 댓글 관계를 추가  
		-------------------------------------------------------------------------------------
	
		@Entity 
		public class Board { 

		    @Id 			
		    @GeneratedValue 
		    Integer board_number;

		    String title;
		    String content;

		    @OneToMany(mappedBy = "board")
		    List<Comment> comments
	    }
		-------------------------------------------------------------------------------------
		- (mappedBy = "board") - 해당 객체들을 소유하고 있는 부모와 양방향간 관계를 가지기 위해서 사용한다. 

	- 댓글용 PagingAndSortingRepository<Comment,Integer> 인터페이스를 추가한다.

	- 어플리케이션 실행시 
		"http://localhost:8080/boards/1/comments"
		"http://localhost:8080/comments"			등의 형태로 댓글이 조회 가능하며 

		"http://localhost:8080/comments/2/board" 	형태로 반대로 서로의 주소에 관한 정보가 연결되어있다는 것을 볼 수 있다.
		(GeneratedValue 어노테이션은 설정을 해주지 않으면 순서를 공유한다.)

8. 특이사항 
	- 여러 객체를 부를때 해당 객체는 하위객체를 가지고 호출이 되지않는다. 
	  게시판 링크로 이동을하면 댓글 정보도 나오지만, 게시판 전체 목록에서는 댓글이 조회가 되지 않기 때문에 이를 월할 경우 Controller를 구현해야한다. 		 
