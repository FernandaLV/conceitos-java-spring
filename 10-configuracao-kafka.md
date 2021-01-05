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

Abaixo as propriedades que definem o endereço do Kafka e tópico. Este exemplo usa a variável de ambiente KAFKA_HOST e o valor *default* considera o Kafka rodando no *localhost* porta 9092:

```properties
# Endereço do Kafka
spring.kafka.bootstrap-servers=${KAFKA_HOST:localhost:9092}
#Topico do kafka
spring.kafka.topic.transactions=meu-topico
```

Para o consumidor, configuramos o formato de serialização da chave e da mensagem/evento, o identificador do grupo e a configuração do modelo de coleta.

Também pode ser configurado os pacotes permitidos para desserialização por uma lista delimitada por vírgulas ou `*` que significa desserializar todos os pacotes.

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

#Pacotes confiaveis para o consumidor
spring.kafka.consumer.properties.spring.json.trusted.packages=${KAFKA_TRUSTED_PACKAGES:*}
```

Para o produtor, configuramos também o formato de serialização da chave e da mensagem/evento.

No exemplo abaixo a chave é formato String, a mensagem no formato JSON.

```properties
# Formato da chave (String) enviada
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer

# Formato da mensagem \ evento (JSON) enviada(o)
spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer
```

## Exemplo de código

*O código completo, com opção de configuração através de classe de configuração invés de arquivo de propriedades esta [neste repositório](https://github.com/FernandaLVItau/estudo-kafka)*

Considere a seguinte entidade de evento de transação:

```java
public class EventoDeTransacao {

    private String id;
    private BigDecimal valor;
    
    /*Construtor, gets e toString*/
}
```

Com as propriedades configuradas conforme acima, segue a classe produtor e consumidor:

### Classe Produtor

```java
@Component
public class ProdutorDeTransacao {

    @Autowired
    private KafkaTemplate<String, EventoDeTransacao> kafkaTemplate;

    @Value("${spring.kafka.topic.transactions}") //Busca valor definido no arquivo application.properties
    private String topicName;
    
    private int numTeste; //Variavel criada para teste

    public void enviarMensagem(EventoDeTransacao evento) {
    
        ListenableFuture<SendResult<String, EventoDeTransacao>> future = kafkaTemplate.send(topicName, evento);

        future.addCallback(new ListenableFutureCallback<SendResult<String, EventoDeTransacao>>() {

            @Override
            public void onSuccess(SendResult<String, EventoDeTransacao> result) {
                System.out.println("Enviado evento=[" + evento +
                        "] com offset=[" + result.getRecordMetadata().offset() + "]");
            }
            @Override
            public void onFailure(Throwable ex) {
                System.out.println("Evento nao enviado=["
                        + evento + "] mensagem da excecao: " + ex.getMessage());
            }
        });
    }

    //Função criada para teste
    @Scheduled(fixedRate = 10000)
    public void produzir() {

        numTeste ++;
        String id = "TESTE_" + numTeste;
        BigDecimal valor = new BigDecimal("10.0");

        EventoDeTransacao evento = new EventoDeTransacao(id, valor);

        enviarMensagem(evento);
    }

}
```

O `KafkaTemplate` envolve uma instância do Produtor e fornece métodos convenientes para enviar mensagens aos tópicos do Kafka.

A API de envio retorna um objeto `ListenableFuture`. Se quisermos bloquear a thread de envio e obter o resultado da mensagem enviada, podemos chamar o get da API deste objeto, a thread vai esperar pelo resultado, mas tornará o produtor lento.

Como o Kafka é uma plataforma de processamento de fluxo rápido, é uma melhor prática tratar os resultados de forma assíncrona para que as mensagens subsequentes não esperem pelo resultado da mensagem anterior. Podemos fazer isso por meio de um *callback* - retorno de chamada

Na função `enviarMensagem` primeiro envia a mensagem utlizando o `KafkaTemplate` e depois adiciona um  *callback* informando o que deve ser realizado em caso de sucesso no `onSuccess` e em caso de falha no `onFailure`.

A função `produzir` é apenas uma função auxiliar criada para realizar teste, criando um novo evento a cada 10.000 ms (10 seg)


### Classe Consumidor

```java
@Component
public class ConsumidorDeTransacao {

    @KafkaListener(topics = "${spring.kafka.topic.transactions}")
    public void ouvir(EventoDeTransacao evento) {
        System.out.println(evento);
    }
}
```

O consumidor é implementado como `@KafkaListener`, neste caso definindo apenas o tópico que ficará escutando, que é notificado sempre que um novo registro é incluído no tópico.

## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://spring.io/projects/spring-cloud-stream](https://spring.io/projects/spring-cloud-stream);
- [https://cloud.google.com/pubsub/docs/overview](https://cloud.google.com/pubsub/docs/overview);
- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-configuration.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-configuration.md);
- [https://howtodoinjava.com/kafka/spring-boot-jsonserializer-example/](https://howtodoinjava.com/kafka/spring-boot-jsonserializer-example/);
- [https://www.baeldung.com/spring-kafka](https://www.baeldung.com/spring-kafka);
- [https://www.baeldung.com/spring-scheduled-tasks](https://www.baeldung.com/spring-scheduled-tasks)
