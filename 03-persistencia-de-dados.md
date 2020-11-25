*Neste documento contém alguns conceitos, classes e anotações utilizadas na persistencia de dados.*

## Serialização e desserialização

Serialização: Converter de outro formato, ex. texto JSON, para objeto;

Desserialização: Converter de objeto para outro formato necessário, ex. texto JSON;

O Spring precisa que as entidades contenham os setters e getters e/ou construtores para realizar as conversões automaticamente, por exemplo, para o controller retornar um objeto, que é uma entidade, e exibir na api o objeto em formato JSON, a entidade precisa ter os métodos getters.


## DTO

*Data Transfer Object* - Objeto de transferência de dados.

A ideia base do DTO é agrupar um conjunto de atributos numa classe simples otimizando a comunicação.

## EntityManager

Classe responsável por gerenciar o ciclo de vida das entidades.

A classe é capaz de:

- Localizar entidades;
- Executar JPQL ou SQL nativo;
- Persistir entidades;
- Atualizar entidades;
- Remover entidades.


## @Transactional

Anotação para realizar transações.

* Inicia a transação; finaliza a transação; trata erros; faz callback.

*Quando usa EntityManage precisa utilizar esta anotação, mas se usar Repository, o repository já tem os tratamentos de transação e não precisa da anotação.*


## @PersistenceContext

Uma instancia **[EntityManager](#entitymanager)** está associado a um contexto de persistencia

Lida com um conjunto de entidades que contêm dados a serem persistidos em, por exemplo, um banco de dados. O Contexto está ciente dos diferentes estados que uma entidade pode ter em relação ao contexto e armazernamento (ex. Banco de Dados).


## JpaRepository

Interface de extenção específica JPA de Repository.

Todas as superinterfaces:
- CrudRepository<T, ID>;
- PagingAndSortingRepository<T,ID>;
- QueryByExampleExecutor<T>;
- Repository<T,ID>.
  
**T**: Tipo do domínio do repositorio (a classe da entidade relacionada ao repositório).

**ID**: Tipo do identificador, por exemplo, Long ou String.


## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://www.devmedia.com.br/serializacao-de-objetos-em-java/23413](https://www.devmedia.com.br/serializacao-de-objetos-em-java/23413);
- [https://pt.stackoverflow.com/questions/31362/o-que-%C3%A9-um-dto](https://pt.stackoverflow.com/questions/31362/o-que-%C3%A9-um-dto);
- [https://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html](https://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html);
- [https://www.baeldung.com/hibernate-entitymanager](https://www.baeldung.com/hibernate-entitymanager);
- [https://www.marcobehler.com/guides/spring-transaction-management-transactional-in-depth](https://www.marcobehler.com/guides/spring-transaction-management-transactional-in-depth);
- [https://www.devmedia.com.br/entendendo-o-java-persistencecontext-extended-e-transient/30493](https://www.devmedia.com.br/entendendo-o-java-persistencecontext-extended-e-transient/30493);
- [https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaRepository.html](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/JpaRepository.html);
- [https://www.baeldung.com/spring-data-repositories](https://www.baeldung.com/spring-data-repositories).
