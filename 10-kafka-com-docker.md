*Neste documento contém as configurações para rodar o kafka utilizando docker e docker-compose*

## Pré-requisitos

1. docker instalado. Recomendado a versão CE, que é a versão open-source da ferramenta;
1. docker-compose instalado.

Para verificar se tem docker instalado na máquia, no terminal:

```shell script
docker container ls
# pode ser necessário utilizar sudo
sudo docker container ls
```

Se o comando rodar com sucesso está com o docker corretamente instalado.

Para verificar se tem o docker-compose instalado na máquina, no terminal:

```shell script
docker-compose version
# pode ser necessário utilizar sudo
sudo docker-compose version
```

Se o comando rodar com sucesso está com o docker-compose corretamente instalado.


De forma resumida, o docker permite que rodemos as aplicações em containers, assim não precisamos instalar a aplicação na nossa máquina; 
já o docker-compose permite agrupar um conjunto de containers e fazendo com que eles "rodem" juntos, criando todas abstrações necessárias como redes, volumes e centralizando em um único manifesto.


## Arquivo docker-compose

Crie um arquivo com o nome `docker-compose.yaml` com o seguinte conteúdo:

```yaml
version: '3'

services:

  zookeeper:
    image: "confluentinc/cp-zookeeper:5.2.1"
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
      ZOOKEEPER_SYNC_LIMIT: 2

  kafka:
    image: "confluentinc/cp-kafka:5.2.1"
    ports:
      - 9092:9092
    depends_on:
      - zookeeper
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: "zookeeper:2181"
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: "1"
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
```

Neste arquivo temos a criação do container do zookeeper, um serviço centralizado para manter informações de configurações e nomenclaturas entre serviços distribuídos que o Kafka utiliza para sincronizar as configurações entre diferentes clusters, e o kafka, configurado para expor na porta 9092, utilizando o zookeeper instanciado no mesmo docker-compose.

Para subir os containers, no terminal, dentro do diretório em que o arquivo está salvo:

```shell script
docker-compose up -d
# pode ser necessário utilizar sudo
sudo docker-compose up -d
```

O parâmetro `-d` é para executar o comando em segundo plano.

Para verificar que os serviços estão executando, no terminal:

```shell script
docker-compose ps
# pode ser necessário utilizar sudo
sudo docker-compose ps
```


## Testando o serviço Kafka no terminal

Em linha de comando, vamos criar um tópico, produzir uma mensagem e comsumi-la.

### Criar tópico

```shell script
docker-compose exec kafka \
kafka-topics --create --topic meu-primeiro-topico --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:2181
# Caso de erro de permissão, utilize o sudo
```

O resultado deve ser:

```shell script
Created topic meu-primeiro-topico.
```

Para visualizar o tópico criado:

```shell script
docker-compose exec kafka  \
  kafka-topics --describe --topic meu-primeiro-topico --zookeeper zookeeper:2181
# Caso de erro de permissão, utilize o sudo
```

O resultado deve ser algo como:

```shell script
Topic:meu-primeiro-topico	PartitionCount:1	ReplicationFactor:1	Configs:
	Topic: meu-primeiro-topico	Partition: 0	Leader: 1	Replicas: 1	Isr: 1
```


### Produzindo mensagem

Com o tópico criado, vamos utilizar um comando para enviar 100 mensagens:

```shell script
docker-compose exec kafka  \
  bash -c "seq 100 | \ 
  kafka-console-producer --request-required-acks 1 --broker-list localhost:29092 --topic meu-primeiro-topico && echo 'Produced 100 messages.'"
# Caso de erro de permissão, utilize o sudo
```

O resultado deve ser:

```shell script
Produced 100 messages.
```

Você também pode executar:

```shell script
docker-compose exec kafka  \
  kafka-console-producer --request-required-acks 1 --broker-list localhost:29092 --topic meu-primeiro-topico
# Caso de erro de permissão, utilize o sudo
```

E então vai permitir que você escreva as mensagem no terminal, cada quebra de linha é uma mensagem nova e para sair utilize `ctrl+c`.


### Consumindo mensagem

Para consumir as 100 mensagens produzidas:

```shell script
docker-compose exec kafka  \
  kafka-console-consumer --bootstrap-server localhost:29092 --topic meu-primeiro-topico --from-beginning --max-messages 100
# Caso de erro de permissão, utilize o sudo
```

O resultado deve ser algo como:

```shell script
1 
....
....
100
Processed a total of 100 messages
```


## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/proposta/001.setup_docker_compose.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/proposta/001.setup_docker_compose.md)
- [https://medium.com/trainingcenter/apache-kafka-codifica%C3%A7%C3%A3o-na-pratica-9c6a4142a08f](https://medium.com/trainingcenter/apache-kafka-codifica%C3%A7%C3%A3o-na-pratica-9c6a4142a08f)
