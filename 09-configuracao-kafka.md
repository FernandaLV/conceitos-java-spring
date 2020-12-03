*Neste documento contém as confugurações básicas para o uso do kafka no Java Spring.*

## Spring Cloud Stream

Spring Cloud Stream é um *framework* para a construção de microsserviços orientados a eventos, altamente escalonáveis, conectados a sistemas de mensagens compartilhados.

O *framework* fornece um modelo de programação flexével construído em uma linguagem Spring já estabalecida e familiar e com as melhores práticas, incluindo suporte para persistencia de semântica pub/sub\*, grupos de consumidores (*consumer group*), e partições com estados (*stateful partitions*).

\* *publisher-subscriber*: O aplicativo de um editor (*publisher*) cria e envia mensagens para um tópico, e para receber essas mensagens, os aplicativos do assinante (*subscriber*) criam uma assinatura de um tópico.


## Adicionar dependências no maven

No arquivo `pom.xml` adicionar a dependência:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-kafka</artifactId>
    <version>3.0.7.RELEASE</version>
</dependency>
```


## Configurações das propriedades

As propriedades são adicionadas no arquivo `application.properties`.

Abaixo a propriedade que define o endereço do Kafka. Este exemplo usa a variável de ambiente KAFKA_HOST e valor *default* considera o Kafka rodando no *localhost* porta 9092:

```properties
# Endereço do Kafka
spring.kafka.bootstrap-servers=${KAFKA_HOST:localhost:9092}
```

Para o consumidor, configuramos o formato de serialização da chave e da mensagem/evento, o identificador do grupo e a configuração do modelo de coleta.

No exemplo abaixo a chave é formato String, a mensagem no formato JSON, o grupo está na variável de ambiente KAFKA_CONSUMER_GROUP_ID com valor *default* minha-aplicacao, e o modelo de coleta na variável de ambiente KAFKA_AUTO-OFFSET-RESET com valor *default* latest:

```properties
# Formato da chave (String) recebida
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer

# Formato da mensagem \ evento (JSON) recebida(o)
spring.kafka.consumer.value-deserializer=org.springframework.kafka.support.serializer.JsonDeserializer

# Identificador do grupo de consumo
spring.kafka.consumer.group-id=${KAFKA_CONSUMER_GROUP_ID:minha-aplicacao}

# Modelo de coleta do consumidor (latest, earliest, none)
spring.kafka.consumer.auto-offset-reset=${KAFKA_AUTO-OFFSET-RESET:latest}
```


Para o consumidor, configuramos também o formato de serialização da chave e da mensagem/evento.

No exemplo abaixo a chave é formato String, a mensagem no formato JSON.

```properties
# Formato da chave (String) enviada
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer

# Formato da mensagem \ evento (JSON) enviada(o)
spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer
```


## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://spring.io/projects/spring-cloud-stream](https://spring.io/projects/spring-cloud-stream)
- [https://cloud.google.com/pubsub/docs/overview](https://cloud.google.com/pubsub/docs/overview)
- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-configuration.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-configuration.md)
- [https://howtodoinjava.com/kafka/spring-boot-jsonserializer-example/](https://howtodoinjava.com/kafka/spring-boot-jsonserializer-example/)
