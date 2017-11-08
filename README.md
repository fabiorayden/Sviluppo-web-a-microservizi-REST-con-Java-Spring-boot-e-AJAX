# Sviluppo-web-a-microservizi-REST-con-Java-Spring-boot-e-AJAX
Materiale del corso UDEMY : Sviluppo web a microservizi REST con Java Spring boot e AJAX

Snippet e risorse per il corso

Sezione 1
-----------------------------------------
Istruzioni installazione JRE 1.8
https://www.java.com/it/download/help/download_options.xml

Istruzioni installazione Maven
https://maven.apache.org/install.html

Istruzioni installazione Git
https://git-scm.com/book/it/v1/Per-Iniziare-Installare-Git

Istruzioni installazione IntelliJIDEA Community Edition
https://www.jetbrains.com/idea/documentation/

Documentazione Spring Framework
https://docs.spring.io/spring/docs/current/spring-framework-reference/pdf/spring-framework-reference.pdf

Documentazione Spring Boot
https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/

Documentazione JPA
https://docs.spring.io/spring-data/jpa/docs/current/reference/pdf/spring-data-jpa-reference.pdf

Ottimo tutorial JPA
https://www.petrikainulainen.net/spring-data-jpa-tutorial/

Strategie per definire i metodi-query
https://www.petrikainulainen.net/programming/spring-framework/spring-data-jpa-tutorial-creating-database-queries-from-method-names/
https://www.petrikainulainen.net/programming/spring-framework/spring-data-jpa-tutorial-introduction-to-query-methods/

Documentazione JWT
https://stormpath.com/blog/jwt-java-create-verify
https://stormpath.com/blog/beginners-guide-jwts-in-java
https://github.com/jwtk/jjwt

Documentazione JSR-303
http://beanvalidation.org/1.0/spec/

Project Lombok
https://projectlombok.org/

Libreria criptaggio password
http://www.jasypt.org/

------------------------------------------

Snippets o link utili:

1. Spring Initializer: https://start.spring.io/

4. Struttura del DB

  nome DB: accountdb (H2 - in Memory DB) il nome non è importante
  
  TABLE users      (String ID, String USERNAME, String PASSWORD, String PERMISSION) 
  TABLE accounts   (String ID, String FK_USER, Double TOTAL) 
  TABLE operations (String ID, Date DATE, Double VALUE, String DESCRIPTION, String FK_ACCOUNT1, String FK_ACCOUNT2)

  nome DB: coupondb (in MySQL)
  
  TABLE coupon      ( (autogenerated int) couponid, String couponcode, String account)
  

------------------------------------------

pom.xml di AccountMicroservice

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.quicktutorials.learnmicroservices</groupId>
	<artifactId>AccountMicroservice</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>AccountMicroservice</name>
	<description>First Microservice in SpringBoot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.7.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
	    <dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-data-jpa</artifactId>
	    </dependency>
	    <dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	    </dependency>

	    <dependency>
		<groupId>com.h2database</groupId>
		<artifactId>h2</artifactId>
		<scope>runtime</scope>
	    </dependency>
	    <dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<optional>true</optional>
		<version>1.16.10</version>
	    </dependency>
	    <dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-test</artifactId>
		<scope>test</scope>
	    </dependency>
	    <dependency>
		<groupId>org.jasypt</groupId>
		<artifactId>jasypt</artifactId>
		<version>1.9.2</version>
	    </dependency>
	    <dependency>
		<groupId>io.jsonwebtoken</groupId>
		<artifactId>jjwt</artifactId>
		<version>0.7.0</version>
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

------------------------------------------

pom.xml di CouponMicroservice

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.quicktutorialz.learnmicroservices</groupId>
	<artifactId>CouponMicroservice</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>CouponMicroservice</name>
	<description>It consumes the rest message of account microservice</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.7.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
			<version>5.1.39</version> <!-- without it gives errors-->
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
			<version>1.16.10</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
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

------------------------------------------

##Metodi dei due Service di AccountMicroservice:##

###LoginService:###
	
   * Optional<User> getUserFromDbAndVerifyPassword(String id, String password)
	* Richiama al suo interno:
	   * userDao.findById(id), encryptionUtils.decrypt(pwd) 
	* che possono entrambe lanciare:
	   * UserNotLoggedException
	
   * String createJwt(String subject, String name, String permission, Date date)     
	* Richiama al suo interno:
	   * JwtUtils.generateJwt(...) 	
	* che può lanciare:
	   * UnsupportedEncodingException
	    
   * Map<String, Object> verifyJwtAndGetData(HttpServletRequest request)
	* Richiama al suo interno:
	   * JwtUtils.getJwtFromHttpRequest(request)
	* che può lanciare:
	   * UserNotLoggedException
	* Richiama anche:
	   * JwtUtils.jwt2Map(jwt)
	* che può lanciare:
	   * UnsupportedEncodingException 
	   * ExpiredJwtException 

###OperationService:###

   * List<Operation> getAllOperationPerAccount(String accountId)
   * List<Account> getAllAccountsPerUser(String userId)
   * Operation saveOperation(Operation operation);







