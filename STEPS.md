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

## GraphQL
1. npm i @nestjs/graphql @nestjs/apollo graphql apollo-server-express
2. localhost:3333/graphql
    * queries
    * mutations