# Curso sobre CI / CD

## Mudanças feitas e porque
- O código no momento diz que apenas a uma branche pode ser modificada, o que não é tão usual no dia a dia de trabalho.
```yaml
on:
  push:
    branches: [ Aula_1 ]
  pull_request:
    branches: [ Aula_1 ]

```
- Colocando o * conseguimos dizer que todas as branches poderão utilizar dos recurso.
```yaml
    on:
    push:
        branches: [ '*' ]
    pull_request:
        branches: [ '*' ]
```
- É sempre importante lembrar que o software pode ser criado para rodar em qualquer sistema operacional(SO), então não é uma boa prática colocar ele para rodar em apenas um.
```yaml
  test:
    runs-on: ubuntu-latest
```
- Podemos utilizar o sistema de matriz  para conseguirmos utilizar em vários so ou em várias versões de um mesmo so.
```yaml
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        go_version: ['1.18', '1.17', '>=1.18']
        os: ['ubuntu-latest', 'ubuntu-18.04']
```
---
## Criando um Dockerfile
- Para criar um arquivo docker precisamos nos atentar para alguns pontos;
- 1 - Qual imagem queremos utilizar;
- 2 - Qual porta ele será exposta;
- 3 - Qual diretorio padrão ele irá criar;
- 4 - O que será copiado para o container;
- 5 - O comando que será executado depois que criar tudo.
```docker
FROM ubuntu:latest

EXPOSE 8000

WORKDIR /app

COPY ./main main

CMD [ "./main" ]
```
---
## Utilizando variáveis de ambientes no GO
- 1 - Importar a biblioteca os
- 2 - Dentro da função utiizar o comando "os.Getenv()" e especificar o que você gostária de pegar no arquivo .env ou similar.

---
## Declarando variáveis de ambiente no dockerfile
- Declare a função ENV e passe as variáveis que você criou, interessante separar o que é variável do banco e o que é do usuário.
```docker
ENV HOST=localhost PORT=5432

ENV USER=root PASSWORD=root DBNAME=root
```

## Declarando variáveis de ambiente no workflow 
- Para que seja possível utilizar variaveis de ambiente automatizadas é necessário criar elas no arquivo .yml.
```yaml
      HOST: localhost
      PORT: 5432
      USER: root
      PASSWORD: root
      DBNAME: 
```

## Referenciando outros arquivos
- Para trabalhar com diversas rotinas é necessário separar elas em arquivdos diferentes e refenciar esses arquivos no final de cada arquivo lido.
```yaml
  docker:
    needs: build
    uses: ./.github/workflows/Docker.yml

```
