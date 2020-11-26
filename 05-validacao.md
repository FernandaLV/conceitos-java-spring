*Neste documento contém alguns conceitos, classes e anotações utilizadas para validações.*

## Anotação Bean Validation

Cria uma anotação para validações, como por exemplo o **@NotBlanck**.

Como criar:

1. Definir a anotação com uma **@interface** (note que utiliza uma @ e não apenas interface);
   1. É necessário os seguintes atributos:
      1. **message** com a mensagem padrão do erro;
      1. **groups** que permite especificar grupos de validação. Por *default* é um *array* vazio do tipo *Class<?>*;
      1. **payload** que pode ser utilizado por clientes da **API Jakarta Bean Validation** para personalizar **payload** de uma restrição. Ela não é usada pela própria API;
   1. É utilizado as seguintes anotações:
      1. **@Target** é informado o alvo da anotação, por exemplo, **Field** (campo);
      1. **@Retention(RUNTIME)** especifica que a nova anotação estará disponível em tempo de execução;
      1. **@Documented** informa que a nova anotação estará contida no JavaDoc;
      1. **@Constraint(validatedBy = {..})** indica qual o validador será utilizado para validar os elementos anotados.
1. Definir o validador;
   1. A classe deve implementar a interface **ConstraintValidator** com dois parâmetros, a anotação criada e o tipo que o validador lida, ex. String;
   1. Deve implementar os dois métodos da interface:
      1. **inicialize** que fornece acesso aos valores de atributo da restrição, permitindo armazena-los em um campo do validador;
      1. **isValid** contém a lógica do validador.

```java
/*
 * Anotação para validar se o id de um registro existe na base de dados.
*/
@Target(FIELD)
@Retention(RUNTIME)
@Documented
@Constraint(validatedBy = {ExistIdValidator.class})
public @interface ExistId {

    String message() default "não cadastrado";
    Class<?>[] groups() default { };
    Class<? extends Payload>[] payload() default { };

    String nomeCampo();         // Será passado na anotação o atribudo nomeCampo com o nome do campo que será validado 
    Class<?> dominioClasse();   // Será passado na anotação o atributo dominioClasse com o domínio da Classe da entidade 
}

/*
 * Validação da anotação ExistId.
*/
public class ExistIdValidator implements ConstraintValidator<ExistId, Long> {
    private String nomeCampo;
    Class<?> dominioClasse;

    @PersistenceContext
    private EntityManager manager;

    @Override
    public void initialize(ExistId constraintAnnotation) {
        nomeCampo = constraintAnnotation.nomeCampo();
        dominioClasse = constraintAnnotation.dominioClasse();
    }

    @Override
    public boolean isValid(Long value, ConstraintValidatorContext constraintValidatorContext) {
        if (value == null) {
            return true;
        }
        
        Query query = manager.createQuery("select 1 from "+dominioClasse.getName()+" where "+nomeCampo+"=:value");
        query.setParameter("value", value);
        List<?> list = query.getResultList();

        return !list.isEmpty();
    }
}
```

Exemplo de uso da anotação criada acima:
```java
@ExistId(dominioClasse = Pais.class, nomeCampo = "id")
private Long idPais;
```

## Assert
Utilizado normalmente em testes, mas pode ser utilizado para validar status ao invés de utilizar *if*.

Se a condição falhar, é executado uma exceção com a mensagem definida.

Exemplo:
```java
Assert.state(list.size() <=1, "Foi encontrado mais de um registro");
```
No exemplo acima, é considerado um erro que executa uma exceção, se na lista tiver mais de um registro.


## Diferença entre @NotNull, @NotEmpty e @NotBlanck

Anotação | Se for nulo | Se estiver vazio | Se estiver vazio depois de aparado
-------- | ------------|------------------|-----------------------------------
**@NotNull** | x | | 
**@NotEmpty** | x | x |
**@NotBlanck** | x | x | x

Nulo: Quando o objeto = null.

Vazio: Quando tamanho/comprimento do objeto = 0, i.e.: String "", Array [], Map {}, Char ''.

Aparado: Remove espaços da String, i.e.: de " teste " fica "teste".


## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://www.baeldung.com/javax-validation](https://www.baeldung.com/javax-validation);
- [https://emmanuelneri.com.br/2017/05/30/criando-validacoes-de-bean-validation-customizadas/](https://emmanuelneri.com.br/2017/05/30/criando-validacoes-de-bean-validation-customizadas/);
- [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/Assert.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/Assert.html);
- [https://www.baeldung.com/spring-assert](https://www.baeldung.com/spring-assert);
- [https://www.baeldung.com/java-bean-validation-not-null-empty-blank](https://www.baeldung.com/java-bean-validation-not-null-empty-blank).
