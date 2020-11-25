## @SpringBootApplication

Esta anotação marca a classe principal de uma aplicação Spring Boot.

```java
@SpringBootApplication
class VehicleFactoryApplication {
 
    public static void main(String[] args) {
        SpringApplication.run(VehicleFactoryApplication.class, args);
    }
}
```


## @Component

É a anotação mais genérica para identificar uma classe como um componente; O Spring automaticamente registra a classe como um Spring Bean.


## @Controller

Indica um tipo específico de componente para classes de controles no Spring MVC de forma mais legivel e apropriada.

Com esta anotação, além de declarar que a classe é um Spring Beans, indica também que é um controle MVC e isto é usado por ferramentas e funcionalidades específicas da web.


## @Repository

Indica um tipo específico de componente para classes da camada de persistencia.


## @Service

Indica um tipo específico de componente para classes com lógica de negócios.


## Quer saber mais?

Abaixo as fontes dos dados deste documento, neles contém mais detalhes e alguns exemplos:

- [https://www.baeldung.com/spring-boot-annotations](https://www.baeldung.com/spring-boot-annotations);
- [https://javarevisited.blogspot.com/2017/11/difference-between-component-service.html](https://javarevisited.blogspot.com/2017/11/difference-between-component-service.html);
- [http://zetcode.com/springboot/component/](http://zetcode.com/springboot/component/)
