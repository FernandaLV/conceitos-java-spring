## O que é Spring?

O Spring é um framework Java criado para facilitar o desenvolvimento de aplicações através dos conceitos de Inversão de Controle e Injeção de Dependências.

Ao utlizar este framework temos recursos, como módulos para persistência de dados, integração, segurança, testes, desenvolvimento web e também possuímos um conceito que nos permite uma solução menos acoplada, mais coesa e, portanto, mais fácil de compreender e mantes.


## Inversão de Controle

Inversão de Controle (IoC - *Inversion of Control*), de forma simples, é um processo no qual um objeto define suas dependencias sem criá-las. Este objeto delega tal tarefa de construir as dependencias a um outro elemento de controle. Este elemento de controle define, por exemplo, como e quando o objeto deve ser criado e quando um método deve ser executado.

Desta forma estas responsabilidades de controle de execução e alguns comportamentos não ficam mais a cargo dos programadores.

No Spring, este elemento de controle é chamado de container, o *Spring Ioc container*.


## Injeção de Dependência

Injeção de Dependência (DI - *Dependency Injection*) é um padrão no qual é implementado o IoC. Com a Injeção de Dependência a classe deixa de se preocupar em como resolver as suas dependências. O ato de conectar objetos com outros objetos, ou “injetar” objetos em outros objetos, é feito por um montador ao invés dos próprios objetos.


## Spring Boot

O Spring Boot é uma ferramenta que tem como objetivo facilitar a configuração e publicação de aplicaçoes que utilizam Spring. Fornecendo a maioria dos componentes basedos no Spring, que são necessários em aplicações no geral, de forma pré-configurada, o Spring Boot possibilita obtermos uma aplicação em produção mais rápido, com mínimo esforço de configuração e implementação. 

O Spring disponibiliza a página [Spring Initializr](https://start.spring.io/). Esta página possibilita habilitar os módulos desejados em seu projeto de forma simples e prático.


## Bean

No Spring, os objetos que formam o *backbone* da aplicação e que são gerenciados pelo Spring IoC container são chamados de *bean*.

- *backbone*: espinha dorsal - sentido figurado: aquilo que serve de base, de sustentação;
- *bean*: grão - sentido figurado: ínfima parcela, dose mínima.

Um *bean* é um objeto instanciado, montado e gerenciado de outro modo, não pelo programador e sim pelo Spring IoC container.  


## Annotation

As *annotarions* (anotações) são uma forma de metadados que fornece dados para o programa, ou seja, elas fornecem informações complementares sobre um programa.

As anotações não fazem parte do aplicativo que desenvolvemos; não tem um efeito direto na operação do código; não alteram a ação do programa compulado.


## Quer saber mais?

Abaixo as fontes dos dados deste documento, neles contém mais detalhes e alguns exemplos:

- [https://www.devmedia.com.br/exemplo/como-comecar-com-spring/73](https://www.devmedia.com.br/exemplo/como-comecar-com-spring/73);
- [https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring);
- [https://www.baeldung.com/spring-bean](https://www.baeldung.com/spring-bean);
- [https://blog.geekhunter.com.br/tudo-o-que-voce-precisa-saber-sobre-o-spring-boot](https://blog.geekhunter.com.br/tudo-o-que-voce-precisa-saber-sobre-o-spring-boot);
- [https://www.javatpoint.com/spring-boot-annotations](https://www.javatpoint.com/spring-boot-annotations).
