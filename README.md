# DIO-Dynamodb
Introdução a utilização do banco de dados Amazon DynamoDB 

## Serviço utilizado
  - Amazon DynamoDB
  - Amazon CLI para execução em linha de comando (Windows)
 
 ## Instalação e configuração
  - Instalar o AWS CLI
  - Download: https://aws.amazon.com/pt/cli/
 
  - Fazer a configuração de acesso pelo prompt
  - (Não esquecer de selecionar a região)
  - Executar primeiro: aws configure
 
## Comandos para execução do experimento:

- Criar uma tabela

```
aws dynamodb create-table --table-name Music --attribute-definitions AttributeName=Artist,AttributeType=S AttributeName=SongTitle,AttributeType=S --key-schema AttributeName=Artist,KeyType=HASH AttributeName=SongTitle,KeyType=RANGE --provisioned-throughput ReadCapacityUnits=10,WriteCapacityUnits=5


