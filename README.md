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
