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
- Para passar os segredos de um arquivo yaml para o outro é necessário utilizar screts
```yaml
  docker:
    needs: build
    uses: ./.github/workflows/Docker.yml
    secrets: inherit
```
---
## Criando um workflow Docker que conecta no docker hub
- Para fazer login no docker hub é necessário criar um conta e passar o usuario e a senha.
- É uma boa pratica de segurança criar um secret passando sua senha ou token do docker hub para não mostrar aos outros usuários suas informações pessoais.

```yaml
name: Docker

on:
  workflow_call:
  
jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: setup Docker Build
      uses: docker/setup-buildx-action@v2.5.0
    
    - name: Docker Login
    # You may pin to the exact commit or the version.
    # uses: docker/login-action@f4ef78c080cd8ba55a85445d5b36e214a81df20a
      uses: docker/login-action@v2.1.0
      with:
    # Username used to log against the Docker registry
        username: purapreguica
    # Password or personal access token used to log against the Docker registry
        password: ${{ secrets.PASSWORD_DOCKER_HUB }}

```
---
## Criando um artefato no workflow
- É preciso criar um artefato para que os dados de um arquivo passem para o outro.
```yaml
 - name: Upload a Build Artifact
      uses: actions/upload-artifact@v3.1.2
      with:
    # Artifact name
        name: api_em_go
    # A file, directory or wildcard pattern that describes what to upload
        path: main
```
---
## Baixando o artefato em outro arquivo
- Para utilizar os dados do artefato anteriormente criado é necessário baixar ele no outro arquivo que está sendo utilizado
```yaml
 - name: Download a Build Artifact
      uses: actions/download-artifact@v2.1.1
      with:
    # Artifact name
        name: api_em_go
```