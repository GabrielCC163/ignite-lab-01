# Ignite Lab 01

- NestJS
- GraphQL
- Apache Kafka
- Next.js
- Apollo Client (GraphQL)
- Auth0

## Funcionalidades

### Serviço de compras (purchases)

- [Admin] Cadastro de produtos
- [Admin] Listagem de produtos

- [Auth] Listagem de compras

- [Public] Compra de um produto
- [Public] Lista produtos disponíveis p/ compra

### Serviço de sala de aula (classroom)

- [Admin] Listar matrículas
- [Admin] Listar alunos
- [Admin] Listar cursos
- [Admin] Cadastrar cursos

- [Auth] Listar cursos que tenho acesso
- [Auth] Acessar conteúdo do curso

----------------------------------------------------------------
# Backend

Purchases (http://localhost:3333/graphql)
Classromm (http://localhost:3334/graphql)

# Frontend

Web

# Apollo Federation
Gateway (http://localhost:3332/graphql)

Enables the front to request only one url (gateway) instead of every backend url.
Enables query from all backend services:

```
query {
    me {
        purchases {
            id
        }

        enrollments {
            id
        }
    }
}
```