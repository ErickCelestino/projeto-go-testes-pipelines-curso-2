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