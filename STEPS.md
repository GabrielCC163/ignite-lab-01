# Purchases
## Auth with Auth0.com
1. Create a new tenant (ignie-node-lab-01)

2. Applications > APIs > + Create API (Auth API)
    * ignite-node-lab-01-auth
    * Algorithm RS256
        * public key (allows token validation)
        * private key (allows token creation)
    * In case of permissions, enable RBAC.

3. Setup .env
    * Create a .env file
    * Add @nestjs/config
    * Import ConfigModule (forRoot()) into http.module.ts 
    * Now we can use process.env.SOMETHING
    * Add the 2 AUTH0 variables

4. Setup the authorizaton.guard.ts with:
    * express-jwt
    * jwks-rsa

5. Setup auth0 into web (frontend)
    * .env
        * AUTH0_SECRET= //openssl rand -hex 32
        * AUTH0_CLIENT_ID=
        * AUTH0_CLIENT_SECRET=
        * AUTH0_BASE_URL=http://localhost:3000
        * AUTH0_AUDIENCE=ignite-node-lab-01-auth //(api) AUTH0_AUDIENCE
        * AUTH0_ISSUER_BASE_URL=https://ignite-node-lab-01.us.auth0.com/ //(api) AUTH0_DOMAIN

    * Create a new application into Auth0.com and setup:
        * Allowed callback urls: http://localhost:3000/api/auth/callback
        * Allowed logout urls: http://localhost:3000/api/auth/logout

## Prisma
1. Add modules
    * npm i prisma -D
    * npm i @prisma/client

2. npx prisma init

3. npx prisma migrate dev

4. Setup prisma.service.ts

5. Access Prisma Studio: npx prisma studio
<!-- Suggestions of use: Prisma-Compositor :: https://github.com/prisma/prisma/issues/2371 -->

## GraphQL
1. npm i @nestjs/graphql @nestjs/apollo graphql apollo-server-express
2. localhost:3333/graphql
    * queries
    * mutations
3. Access GraphQL Playground: /graphql

4. Query example:
In this way the client can make two requests in one:

```
query {
    purchases {
        id
        status
        createdAt

        product {
            slug
            id
        }
    }
}
```

## Apache Kafka (could use RabbitMQ)
In this scenario, we are using Kafka to notify the classroom service that a purchase has been made so that the enrollment can be created for the student.

-> Pub/Sub.

Access UI: localhost:8080

2. npm i @nestjs/microservices kafkajs
3. nest g module messaging
4. Add KAFKA_BROKERS=localhost:29092 to .env
5. Configure kafka.service.ts and use the emit method.
6. To emit a msg is required that the topic must already be created:
    - option 1:

    kafka.service.ts
    
    ```ts
        super({
            ...,
            producer: {
                allowAutoTopicCreate: true
            }
        })
    ```

    - option 2:

    Kafka UI: add a topic. Ex: purchases.new-purchase with 4w (1 month) for time to retain data.

Note: the purchase service will be the emitter (pub) and classroom will be the receiver (sub).
7. Configure classroom > main.ts and purchase.controller.ts


## Apollo Federation >> Gateway

1. Install Apollo Federation into purchases service
Takes all APIs and generates one.

```
purchases > npm i @apollo/federation
```

Then, setup @ObjectType and @Directive into purchases > customer.ts.

Set the "owner" of user into purchases > customers.resolver.ts by using @ResolveReference()

Setup the @Directive() and authUserId into classroom > student.ts.

2. Create gateway project:
```
nest new gateway
```

Add: npm i @apollo/gateway @nestjs/graphql @nestjs/apollo apollo-server-express

Note: purchases and classroom services require to be with graphql 15 to be working with apollo federation.

3. Access localhost:3332/graphql and now we can query from both APIs:
```
query {
    me {
        authUserId

        enrollments {
            course {
                title
            }
        }

        purchases {
            id
            createdAt
        }
    }
}
```