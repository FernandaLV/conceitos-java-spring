*Neste documento contém alguns conceitos, classes e anotações utilizadas na camada de controle no MVC.*

## @RestController

Marca que a classe é um controlador (*controller*) no qual cada método retorna um objeto de domínio em vez de uma visualização (*view*).

Esta anotação é um 'atalho' do uso das anotações:

- **@Controller** - um tipo específico de compomente, o qual permite que a classe seja detectada automaticamente;
<a name="responsebody"><a/>
- **@ResponseBody** - indica que o(s) método(s) retorna(m) um valor associado ao corpo de uma resposta da web.


## @Autowired

Marca um construtor, campo, método setter ou método de configuração para ser autocarregado pelos recursos de injeção de dependência do Spring.


## @InitBinder

Funciona como um pré-processador para cada requisição feita para o controller.


## WebDataBinder

Um tipo específico para requisição web de DataBinder, que permite definir propriedades para serem pré-processados na requisição.

Por exemplo, adicinar para realizar algumas validações antes de processar a requisição.


## Adicionar validação no InitBinder

```java
@Autowired
Validador1 validador1;

@InitiBinder
public void init(WebDataBinder binder) {
  binder.addValidators(validado1, new validador2());
}
```

Ambas as formas podem ser passadas como parâmetro para `addValidators`. Se a classe que realiza a validação tiver propriedades que usam Sprint para inicializar, como por exemplo **[@Autowired](#autowired)**, deve realizar da forma do `validador1`.

As classes de validação devem implementar **Validator** do *org.springframework.validation.Validator*.


## @ExceptionHandler

Anotação para lidar com exeções em classes e/ou métodos manipuladores (*handlers*) específicos.


## @ControllerAdvice

Tipo específico de componente para classes com métodos **[@ExceptionHandler](#exceptionhandler)**, **[@InitBinder](#initbinder)** ou **@ModelAttibute** a serem comartilhados entre vários controladores (*controllers*)


## @RestControllerAdvice

Os tipos com esta anotação são tratados como advice do controlador, onde métodos **[@ExceptionHandler](#exceptionhandler)** assumem a semântica **[@ResponseBody](#responsebody)**.

Esta anotação é um 'atalho' do uso das anotações:

- **[@ControllerAdvice](#controlleradvice)**;
- **[@ResponseBody](#responsebody)**.


## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html);
- [https://www.baeldung.com/spring-controller-vs-restcontroller](https://www.baeldung.com/spring-controller-vs-restcontroller);
- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html);
- [https://medium.com/stackavenue/how-to-use-initbinder-in-spring-mvc-ecb974a6884](https://medium.com/stackavenue/how-to-use-initbinder-in-spring-mvc-ecb974a6884);
- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/WebDataBinder.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/WebDataBinder.html);
- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/Validator.html);
- [https://www.baeldung.com/exception-handling-for-rest-with-spring](https://www.baeldung.com/exception-handling-for-rest-with-spring);
- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html).
