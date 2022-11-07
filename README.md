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

- Criar tabela
```
aws dynamodb create-table --table-name Music --attribute-definitions AttributeName=Artist,AttributeType=S AttributeName=SongTitle,AttributeType=S --key-schema AttributeName=Artist,KeyType=HASH AttributeName=SongTitle,KeyType=RANGE --provisioned-throughput ReadCapacityUnits=10,WriteCapacityUnits=5

```

- Inserir um item
```
aws dynamodb put-item --table-name Music --item file://itemmusic.json

```

- Inserir múltiplos itens
```
aws dynamodb batch-write-item --request-items file://batchmusic.json
```

- Criar um index global secundário baeado no título do álbum
```
aws dynamodb update-table --table-name Music --attribute-definitions AttributeName=AlbumTitle,AttributeType=S --global-secondary-index-updates "[{\"Create\":{\"IndexName\": \"AlbumTitle-index\",\"KeySchema\":[{\"AttributeName\":\"AlbumTitle\",\"KeyType\":\"HASH\"}], \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

- Criar um index global secundário baseado no nome do artista e no título do álbum
```
aws dynamodb update-table --table-name Music --attribute-definitions AttributeName=Artist,AttributeType=S AttributeName=AlbumTitle,AttributeType=S --global-secondary-index-updates "[{\"Create\":{\"IndexName\": \"ArtistAlbumTitle-index\",\"KeySchema\":[{\"AttributeName\":\"Artist\",\"KeyType\":\"HASH\"}, {\"AttributeName\":\"AlbumTitle\",\"KeyType\":\"RANGE\"}], \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

- Criar um index global secundário baseado no título da música e no ano
```
aws dynamodb update-table --table-name Music --attribute-definitions AttributeName=SongTitle,AttributeType=S AttributeName=SongYear,AttributeType=S --global-secondary-index-updates "[{\"Create\":{\"IndexName\": \"SongTitleYear-index\",\"KeySchema\":[{\"AttributeName\":\"SongTitle\",\"KeyType\":\"HASH\"}, {\"AttributeName\":\"SongYear\",\"KeyType\":\"RANGE\"}], \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

- Pesquisar item por artista
```
aws dynamodb query --table-name Music --key-condition-expression "Artist = :artist" --expression-attribute-values file://keyconditions2.json 
```

- Pesquisar item por artista e título da música
```
aws dynamodb query --table-name Music --key-condition-expression "Artist = :artist and SongTitle = :title" --expression-attribute-values file://keyconditions1.json
```

- Pesquisa pelo index secundário baseado no título do álbum
```
aws dynamodb query --table-name Music --index-name AlbumTitle-index --key-condition-expression "AlbumTitle = :name" --expression-attribute-values file://keyconditions3.json  
```

- Pesquisa pelo index secundário baseado no nome do artista e no título do álbum
```
aws dynamodb query --table-name Music --index-name ArtistAlbumTitle-index --key-condition-expression "Artist = :v_artist and AlbumTitle = :v_title" --expression-attribute-values file://keyconditions4.json
```

- Pesquisa pelo index secundário baseado no título da música e no ano
```
aws dynamodb query --table-name Music --index-name SongTitleYear-index --key-condition-expression "SongTitle = :v_song and SongYear = :v_year" --expression-attribute-values file://keyconditions5.json
```

- Deletar tabela
```
aws dynamodb delete-table --table-name Music
```
