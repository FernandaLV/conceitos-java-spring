*Neste documento contém algumas anotações usadas em entidades.*

## @GeneratedValue

Esta anotação é utilizada após **@Id** (mostra que o atributo é um identificador único) para indicar que será responsabilidade do provedor de persistência gerenciar o identificador único (chave). Sem esta anotação, o gerenciamento dica na responsabilidade da aplicação.

É possível definir a estratégia de geração da chave pelo atributo *strategy*, as opções são:

- **GenerationType.AUTO**: Padrão. O provedor de persistência escolhe a estratégia mais adequada conforme o Banco de Dados;
- **GenerationType.IDENTITY**: Informa ao provedor de persistência que as chaves serão geradas pelo autoincremento do Banco de Dados. Alguns Banco de Dados podem no suportar essa opção;
- **GenerationType.SEQUENCE**: Informa ao provedor de persistência que as chaves serão geradas a partir de uma sequencia. A sequencia pode ser especificada pelo atributo *generator*, caso não seja, é usada uma padrão, global, para todas as entidades. Alguns Banco de Dados podem no suportar essa opção;
- **GenerationType.TABLE**: Informa ao provedor de persistência que as chaves serão geradas a partir de uma tabela, que será criada para gerenciar as chaves. Isso gera uma sobrecarga de consultas para manter a tabela atualizada e, por isso, é a opção menos recomendada.


## @Enumerated

 Indica que o campo é um *enum* e pode especificar se deve persistir o valor inteiro do *enum* (*default*, não precisa passar argumento), ou a descrição utilizando **@Enumerated(EnumType.String)**


## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://www.devmedia.com.br/jpa-como-usar-a-anotacao-generatedvalue/38592](https://www.devmedia.com.br/jpa-como-usar-a-anotacao-generatedvalue/38592);
- [https://docs.oracle.com/javaee/6/api/javax/persistence/Enumerated.html](https://docs.oracle.com/javaee/6/api/javax/persistence/Enumerated.html)
