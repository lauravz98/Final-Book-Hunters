# Book-Hunters
Welcome to the BookHunters application. Created by [**Laura Zambrano**](https://github.com/lauravz98) 

This application is the final project of the FullStack JAVA Bootcamp powered by Ironhack - Accenture.

# Table of contents
1. [What is the book hunter?](https://github.com/lauravz98/Final-Book-Hunters#whats-book-hunters)
2. [Project overview](https://github.com/lauravz98/Final-Book-Hunters#project-overview)
3. [Technical features](https://github.com/lauravz98/Final-Book-Hunters#technical-features)
4. [Getting started](https://github.com/lauravz98/Final-Book-Hunters#getting-started)
5. [Next steps](https://github.com/lauravz98/Final-Book-Hunters#next-steps)
6. [Acknowledgements](https://github.com/lauravz98/Final-Book-Hunters#acknowledgements)

# What's Book Hunters?

Book Hunters is an application created with the aim of creating a collaborative community of readers where books are shared, through the app that connects people who want to hide their books and others who want to find them.

<p align="center">
    <img src = /img/3.png width="650">
</p>
In this sense, a cycle is created where the useful life of the books does not end and reading is stimulated in a playful way.


# Project overview
In this project different technologies were used for the back and front end, especially spring-boot and angular. The global architecture diagram of the project is as follows.

<p align="center">
    <img src = /img/BookHunter.drawio.png width="650">
</p>

### Frontend technologies
The Front was developed in Angular, using mainly TS, HTML and CSS. We used libraries such as Angular Material for the components, Bootstrap and Toastify.
<p align="center">
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/angularjs/angularjs-original.svg" width="120" height="120" />	
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" width="120" height="120"/>	   
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original-wordmark.svg" width="120" height="120"/>           
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original-wordmark.svg" width="120" height="120"/>  
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/bootstrap/bootstrap-original.svg" width="120" height="120"/>
</p>  

### Backend technologies
Developed the backend with microservices with Java Spring Boot mainly. And a microservice in Python with flask. Mysql was also used for the database, and RDS from Amazon Web Services.
<p align="center">
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original-wordmark.svg"  width="120" height="120"/>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/spring/spring-original-wordmark.svg" width="120" height="120"/>          
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original-wordmark.svg" width="120" height="120"/>
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original-wordmark.svg" width="120" height="120"/>
	<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" width="120" height="120"/>          
</p>       

          
# Technical features
* The core of the application consists of 3 microservices proxies, books, review and keywords, and an Edge service in charge of the authentication and security of the application. Spring-boot services have their respective database.

<p align="center">
    <img src = /img/back_core.png width="650">
</p>

* The use-case diagram of the actions that a reader user can perform is as follows. In short, users can create a new account, log in, consult books, add them to their library, view their books, both general and hidden and hunted, hunt books, hide them, and view the general list of books to hunt and hunted.

<p align="center">
    <img src = /img/casos_uso.png>
</p>

* The entity-relationship diagram is as follows. From the entity book inherit the tables of my books, hidden books and found books.

<p align="center">
    <img src = /img/uml.png width="650">
</p>

* For the connection with the MySQL database, the data management with SpringBoot and the Tomcat web server the following dependencies were used:


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
		</dependency>
    
* The local communication was done through a eureka server that communicated the different microservices, so it was necessary to register them with eureka clients. Open Feign was also used for the services that use other microservices, for example the EdgeService or ReviewsService.
    
<p align="center">
    <img src = /img/eureka2.png width="650">
</p>

* The security was done with Spring Security. For this we used the ``spring-boot-starter-security`` dependency in the Edge Service. And we added the security configuration for the private routes, as well as a Custom User Details, and its service.
		
```java
@Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.httpBasic();
        http.csrf().disable();
        http.authorizeRequests()
                .antMatchers(HttpMethod.GET, "/myBooks/**", "/bookFound/**", "/bookHidden/**"
			, "/login").authenticated() 
                .antMatchers(HttpMethod.POST, "/myBooks/**").authenticated()
                .antMatchers(HttpMethod.PUT, "/myBooks/**").authenticated()
                .antMatchers(HttpMethod.DELETE, "/myBooks/**").authenticated()
                .anyRequest().permitAll(); // Others endpoints son public
        return http.build();
    }

```

* Unit and integration tests were performed for the microservices. Obtaining a line coverage of 85% for unit tests. And 90% of the curved methods for the Edge Service, omitting the Circuit Breaker.

<p align="center">
    <img src = /img/cobertura_tests.png >
</p>
    
* Additionally, the configuration of the mentioned microservices was externalized using the ``spring-cloud-starter-config`` dependency. The [configuration repository](https://github.com/lauravz98/Book-Hunter-Config-repo.git) is publicly available on Github.
	
   
<p align="center">
    <img src = /img/config_repo.png >
</p>

* The Circuit Breaker design pattern was also used to handle error handling. For this purpose, the following dependencies were added  ```spring-cloud-starter-circuitbreaker-resilience4j``` and ```spring-cloud-starter-bootstrap```

* The application was deployed in heroku both front and back end, except for the python keyword service for compatibility. The link to access the front is [this](https://book-hunter-front.herokuapp.com/) 

<p align="center">
    <img src = /img/heroku.png >
</p>

> Note: this repository corresponds to the pre-deployment version because we had to reduce the services because heroku only supported 4 deployed services.

* The application databases were connected to an AWS RDS instance. For privacy reasons, AW credentials are not included.
<p align="center">
    <img src = /img/AWS.png >
</p>

* Also included is the postman's collection of the most common requests. The following table summarizes some of the requests that can be made on the Edge service



| Method | Endpoint                     | Params                     | Description                   |
|--------|------------------------------|----------------------------|-------------------------------|
| GET    | /myBooks                     | userId: number             | Get books by user             |
| GET    | /myBooks/{book_id}           | userId: number             | Get a book by user            |
| POST   | /myBooks/{book_id}           | userId: number, Body: Book | Add a book to user's library  |
| PUT    | /myBooks/{book_id}/hide      | userId: number             | Hide a book by user           |
| DELETE | /myBooks/{book_id}           | userId: number             | Delete a book by user         |
| GET    | /myBooks/bookHidden          | userId: number             | Get hidden books by user      |
| GET    | /myBooks/bookFound           | userId: number             | Get hunted books by user      |
| PUT    | /myBooks/bookFound/{book_id} | userId: number             | Hunt a book by user           |
| GET    | /bookHidden                  | None                       | Get all hidden books          |
| GET    | /bookFound                   | None                       | Get all hunted books          |
| GET    | /books/{book_id}/keywords    | None                       | Get keywords from a book      |
| GET    | /login                       | Auth: UserDetails          | Login in the app              |
| POST   | /user                        | Body: UserDTO              | Create a new user             |
    
# Getting Started

1. Clone each project: ```git clone <project_name>``` . Or download ZIP
2. Create schemas in MySQL for the Database of the projects: Books, Reviews and Edge. The DDL file are on the path ```<project_name>/src/main/resources/DDL.sql```
3. Run the microservices with ```mvn spring-boot:run``` . The suggested order is:
```
https://github.com/lauravz98/Book-Hunter-Config-Server
https://github.com/lauravz98/Eureka-Server 	
https://github.com/lauravz98/Book-Hunter-Back-Proxy-Books
https://github.com/lauravz98/Book-Hunter-Back-Proxy-Keywords
https://github.com/lauravz98/Book-Hunter-Back-Edge
```
To run the keyword service in python, open the folder with VSCode and run in the terminal:
```
reviews-service/Scripts/activate.bat
python flask_app.py
```
> the first command activates the virtual environment and the second one executes the python file. You may need to have python installed on your computer and it may ask you to install some libraries. In case it is necessary ```pip install flask pandas py-eureka-client ```

4. Open the frontend and execute the following commands
```
npm install 
ng serve 
```

Now bookhunters is running successfully on your computer locally in http://localhost:4200/. Good luck!

# Next Steps
* Location with Google API
* Full Deployment
* Add more classes, authors
* Advanced Search
* Dark Mode
* Advanced NPL algorithms
*Include more APIs
* Mobile version, with AR...

# Acknowledgements

Thank you very much to the Ironhack team for this incredible training. And Accentrure for this great opportunity.
