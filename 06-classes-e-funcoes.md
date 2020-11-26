*Neste documento contem alguns conceitos de classes, coleções e funções.*

## BigDecimal

As casas decimais são mais precisas que o **double**, portanto mais recomendado para trabalhar com valores monetários.

A desvantagem é no uso para operações.


## Diferença entre Set, List e Map

Todas são interfaces do pacote de coleções (*collections*):

- **Set**: Não armazena a posição em que estão os elementos, ou seja, ela entende que as classes que as implementam so como conjuntos matemáticos cuja posição não é relevante. Não permite objetos iguais no conjunto;
- **List**: Armazena a posição dos elementos, possibilitando buscar pelo índice do elemento. Permite objetos iguais na lista;
- **Map**: Armazena pares de chave-valor. Não pode haver chaves iguais.

## .stream()

Converte uma coleção para uma interface do tipo **Stream** e dela podemos realizar operações **Filter**, **Map** e **Reduce**.

Esta interface permite chamar um método depois do outro, ou seja, de forma encadeara.

No exemplo abaixo, filtra dados de uma lista, mapea os dados do objeto para números inteiros e por fim realiza a somatória destes valores mapeados:

```java
lista.stream().filter(...).mapToInt(...).sum()
```


## Lambda e método por referência

Considere o exemplo em que há uma lista de objetos `Livro1` que será convertida para uma lista de objetos `Livro2`. A classe `Livro2` tem um construtor que recebe um objeto `Livro1` como parâmetro.

-> Código 1 - Utlizando laço *for*
```java
for(Livro livro : listaLivro) {
  lista2Livro[] = new Livro2(livro);
}
```

-> Código 2 - Utilizando Lambda
```java
lista2Livro = listaLivro.stream().map(livro -> { return new Livro2(livro); });
```

-> Código 3 - Utilizando método por referência
```java
lista2Livro = listaLivro.stream().map(Livro2::new);
```

Lambda é uma função anônima.

Uma expressão lambda pode ser substituda por uma referência, o argumento é obtido por inferência.


## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://www.devmedia.com.br/java-bigdecimal-trabalhando-com-mais-precisao/30286](https://www.devmedia.com.br/java-bigdecimal-trabalhando-com-mais-precisao/30286);
- [https://docs.oracle.com/javase/7/docs/api/java/math/BigDecimal.html](https://docs.oracle.com/javase/7/docs/api/java/math/BigDecimal.html);
- [https://www.techiedelight.com/difference-between-list-set-map-interface-java/](https://www.techiedelight.com/difference-between-list-set-map-interface-java/);
- [https://beginnersbook.com/2015/01/difference-between-list-set-and-map-in-java/](https://beginnersbook.com/2015/01/difference-between-list-set-and-map-in-java/);
- [https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html);
- [https://www.baeldung.com/java-8-streams](https://www.baeldung.com/java-8-streams);
- [https://www.devmedia.com.br/como-usar-funcoes-lambda-em-java/32826](https://www.devmedia.com.br/como-usar-funcoes-lambda-em-java/32826);
- [https://www.baeldung.com/java-method-references](https://www.baeldung.com/java-method-references).
