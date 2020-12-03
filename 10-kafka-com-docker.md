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
```

## Quer saber mais?

Abaixo as fontes dos dados deste documento e documentações com mais detalhes e exemplos:

- [https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/proposta/001.setup_docker_compose.md](https://github.com/zup-academy/nosso-cartao-documentacao/blob/master/proposta/001.setup_docker_compose.md)
