*Neste documento contém conceitos iniciais do kafka.*

## Event streaming

Streaming de eventos, na prática, garante um fluxo contínuo e a interpretação dos dados para que as informações certas estejam no lugar certo e na hora certa. O streaming de eventos garante:

1. Capturar dados em tempo real de fontes como bancos de dados, sensores, dispositivos móveis, serviços em nuvem e aplicativos de software na forma de fluxos de eventos;
1. Armazenar esses fluxos de eventos de forma duradoura para recuperar mais tarde;
1. Manipular, processar e reagir aos fluxos de eventos em tempo real e também retrospectivamente;
1. Encaminhar os fluxos de eventos para diferentes tecnologias de destino, conforme necessário.


 ## Apache Kafka
 
Apache Kafka é uma plataforma de streaming de eventos distribuída de código aberto. 

O Kafka combina três recursos chaves que podemos implementar para casos de uso de streaming de eventos de ponta a ponta:

- Publicar (gravar) e aceitar (ler) fluxos de eventos, incluindo importação / exportação contínua de seus dados de outros sistemas;
- Armazenar fluxos de eventos de forma durável e confiável pelo tempo desejado;
- Processar fluxos de eventos conforme eles ocorrem ou retrospectivamente.

Estas funcionalidades são todas fornecidas de forma distribuída, altamente escalável, elástica, tolerante a falhas e de forma segura.  O Kafka pode ser implantado em *hardware bare-metal* (sistema de computador sem sistema operacional base ou aplicações instaldas), máquinas virtuais e contêineres; podem ser implementadas no local e também na núvem. Podemos escolher entre autogerenciamento do ambiente Kafka ou o uso de serviços gerenciados oferecidos por uma variedade de fornecedores.


## Modelos de entregas

Há dois tipos de modelos de entrega:

- ***Push model***: os dados são enviados para os consumidores.
- ***Pull model***: os dados são coletados pelos consumidores.

O Apache Kafka utiliza o *pull model*, trazendo algumas vantagens:

- Consumidores conseguem controlar a quantidade de eventos que buscam, reduzindo as operações na rede.
- Consumidores conseguem controlar quais mensagens querem consumir, por exemplo, todas da última semana, ano, mês.
- Apache Kafka não precisa controlar os envios de eventos.


## Fila e Tópico

Os dois padrões de mensagerias mais comuns são fila (*queue*) e tópico (*topic*):

- **Fila**: As mensagens são incluidas e lidas em ordem sequencial por quem escreve a mensagem e quem lê a mensagem, respectivamente. É enviado uma mensagem e o receptor as recebe na ordem enviada.
- **Tópico**: Semelhante a uma fila, entretando, em vez de um receptor, no tópico temos o conceito de *broadcast*. Um produtor envia mensagens para um tópico e as partes interessadas na informação assinam este tópico e passam a receber as mensagens enviadas.

O Kafka utiliza o conceito de tópico e partição. O tópico é como categoriza os grupos de mensagens, por exemplo, transações. Neste tópico sabemos que contem os dados de transações, e este tópico pode conter várias partições.


## Partição

Para manter a ordenação em um ecossistema de Kafka, os tópicos possuem partições e fatores de replicação. Um tópico pode possuir uma ou mais partições. Ao receber uma nova mensagem o Kafka automaticamente direciona aquela mensagem para uma partição específica, dependendo de sua chave (*key*). Desta forma as mensagens de uma mesma chave estarão apenas em uma única partição, e garante a leitura ordenada de todas as mensagens de um tópico dentro da partição.

![particoes](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/images/kafka-004.png)

 Dentro de cada partição as mensagens se mantém ordenadas. Por exemplo, se quer manter ordenada as transações por estabelecimento, poderia ter uma chave de partição para cada estabelecimento.
 
 
 ## Cluster

![cluster](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/images/kafka-001.png)

O *broker* é o coração do ecossistema do Kafka. Um Kafka Broker é executado em uma única instância em sua máquina. Um conjunto de *brokers* entre diversas máquinas formam um Kafka Cluster.
Uma das principais características do Kafka é a escalabilidade e resiliência que ele oferece. É possível ter o Kafka localmente na sua própria máquina e esta teria um Kafka Broker formando um Kafka Cluster, como também subir diversas instancias de Kafka Brokers e todas estarem no mesmo Kafka Cluster. Assim, podemos escalar a aplicação e replicar os dados entre os *brokers*.
 
 
 ## Producer
 
 Os produtores (*producer*) são os que escrevem as mensagens e alimentam os tópicos. O tópico pode ter vários produtores.
 
Para enviar um evento é preciso enviar as seguintes informações:

- Nome do tópico;
- Corpo do evento, geralmente enviado no formato JSON;
- Chave (opcional), caso deseje que todas as mensagens sejam enviadas para uma mesma partição e de forma ordenada.

O produtor envia os eventos para o *broker* líder, ou seja, em um conjunto de máquinas (*brokers*) que formam o *cluster* Kafka, é eleito um líder que irá receber os eventos de acordo com o tópico e partição. No Apache Kafka não existe um líder geral e sim um líder específico por partição.

Para que seja possível saber os líderes o Apache Kafka informa para os produtores quais *brokers* estão aptos a receber eventos. É importante ter replicações das partições, pois, se uma máquina (*broker*) cai a outra assume a liderança da(s) partição(ões).

Para o produtor reconhecer o envio do evento (*acks*), há três configurações:

- **0**: O produtor não irá esperar pelo reconhecimento do recebimento do evento pelo Apache Kafka;
- **1**: O produtor somente irá esperar pelo reconhecimento do recebimento do evento pelo líder;
- **all**: O produtor irá esperar pelo reconhecimento do recebimento do evento por todos os *brokers* que tenha a partição.

Desta forma, se deseja não perder nenhuma mensagem utilize o modelo de reconhecimento do tipo *all*, que garante que o evento foi recebido pelo lider **e** replicado.


## Consumer

Os consumidores (*consumers*) são os que processam os eventos/mensagens de um determinado tópico, em uma ou mais partição, e é de sua responsábilidade gravar qual ponto parou de ler e em qual partição.

Caso haja necessidade, é possível processar os eventos novamente, basta o consumidor zerar seu histórico de processamento (*offset*), pois o Apache Kafka armazena os eventos de cada partição de acordo com o configurado (dia, mês, ano, etc).

Para que isso seja possível o consumidor precisa configurar qual modelo de coleta de eventos ele quer fazer:

- ***latest***: Processa a partir do último processado.
- ***earliest***: Zera o offset e processa desde o início.
- ***none***: Não processa nenhum evento e lança uma exceção em sua aplicação.

O Apache Kafka tem o conceito de grupo (*consumer group*), que tem a responsabilidade de balancear a carga de trabalho com a quantidade de partições.

Todo consumidor no Apache Kafka deve pertencer a um grupo, e o controle de histórico de processamento é por grupo e partição. O modelo de escalabilidade do consumidor está atrelado a quantidade de consumidor x partições.

No caso em que há um consumidor no grupo, para processar o evento de três as partições, este consumidor irá receber de todas as repartições:

![um_consumidor](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/images/kafka-005.png)

Se houver dois consumidores no mesmo grupo para processar três partições de um tópico, um consumidor irá processar de uma partição e o outro as outras duas partições:

![dois_consumidores](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/images/kafka-006.png)

Caso tenha três consumidores no memso grupo para processar três partições, cada consumidor irá processar uma partição:

![tres_consumidores](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/images/kafka-007.png)

E se tivermos quatro consumidores no mesmo grupo para processar três partições, um consumidor ficará sem atividade, ou seja, ocioso:

![quatro_consumidores](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/images/kafka-008.png)


 ## Quer saber mais?
 
Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:
 
- [https://kafka.apache.org/intro](https://kafka.apache.org/intro);
- [https://kafka.apache.org/documentation/#design_pull](https://kafka.apache.org/documentation/#design_pull);
- [https://medium.com/@gabrielqueiroz/o-que-%C3%A9-esse-tal-de-apache-kafka-a8f447cac028](https://medium.com/@gabrielqueiroz/o-que-%C3%A9-esse-tal-de-apache-kafka-a8f447cac028);
- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_procedural/kafka.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_procedural/kafka.md)
- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-partition.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-partition.md)
- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-producer.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-producer.md)
- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-consumer.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/informacao_suporte/kafka-consumer.md)
 
