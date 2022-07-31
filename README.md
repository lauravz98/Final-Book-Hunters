# Book-Hunters
Bienvenido a la applicacion de BookHunters. Creado por [**Laura Zambrano**](https://github.com/lauravz98) 

Esta aplicacion es el proyecto final del Bootcamp de FullStack de JAVA powered by Ironhack - Accenture.

# Contenido
1. What's Book Hunters?
2. About the project
3. Next steps
4. Acknowledgments


Desarrollada el back con microservicios con Java Spring Boot principalmente. Y un microservicio en Python con flask
El Front fue desarrollado en Angular, utilizando principalmente TS, HTML y CSS. Se utilizaron librerias como Angular Material para los componentes, Bootstrap y Toastify.

<img src="https://github.com/FortAwesome/Font-Awesome/blob/6.x/svgs/brands/angular.svg" width="30" height="30"> 

<img src="https://github.com/FortAwesome/Font-Awesome/blob/6.x/svgs/brands/java.svg" width="30" height="30"> 

### <img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/crown.svg" width="20" height="20"> Caracteristicas tecnicas:
Para la conexion con la base de datos en MySQL, el manejo de datos con SpringBoot y el servidor web Tomcat se utilizaron las siguientes dependencias:


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
    
La comunicacion a nivel local se realizo mediante un servidor de eureka que comunicaba los distintos microservicios, por lo que fue necesario registrarlos con eureka clients. Tambien se utilizo Open Feign para los servicios que utilizan a otros microservicios, por ejemplo el EdgeService o ReviewsService.
    
<p align="center">
    <img src = /img/eureka2.png width="650">
</p>

* La seguridad se hizo con Spring Security. Para ello se utilizo la dependencia de ```spring-boot-starter-security``` en el Servicio Edge. Y se añadio la configuración de la seguridad para las rutas privadas, asi como un Custom User Details, y su service.
		
```java
@Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.httpBasic();
        http.csrf().disable();
        http.authorizeRequests()
                .antMatchers(HttpMethod.GET, "/myBooks/**", "/bookFound/**", "/bookHidden/**", "/login").authenticated() 
                .antMatchers(HttpMethod.POST, "/myBooks/**").authenticated()
                .antMatchers(HttpMethod.PUT, "/myBooks/**").authenticated()
                .antMatchers(HttpMethod.DELETE, "/myBooks/**").authenticated()
                .anyRequest().permitAll(); // Others endpoints son public
        return http.build();
    }

```

* Se realizaron test unitarios y de integracion para los microservicios. Obteniendo una cobertura de linea del 85% para las pruebas unitarias. Y un 90% de los metodos curbiertos para el Edge Service, omitiendo el Circuit Breaker.

<p align="center">
    <img src = /img/cobertura_tests.png >
</p>
    
* Adicionalmente se realizo la externalizacion de la configuracion de los microservicios mencionados, utilizando la dependencia ```spring-cloud-starter-config```. El [repositorio de la configuracion](https://github.com/lauravz98/Book-Hunter-Config-repo.git) se encuentra publico en Github.
	
   
* Tambien se utilizo el patron de diseño Circuit Breaker para manejar el control de errores. Para ello se añadieron las dependencias de ```spring-cloud-starter-circuitbreaker-resilience4j``` y ```spring-cloud-starter-bootstrap```
    
    
