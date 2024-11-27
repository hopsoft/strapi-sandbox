# Strapi Sandbox

This project is a sandbox for testing Strapi features, plugins, etc.

It mimics 2 environments:

- Production: http://localhost:1337
- Staging: http://localhost:1338

This setup supports experimenting with various commands that handle transfering data between environments.

## Usage

1. Clone the repository

   ```bash
   git clone https://github.com/hopsoft/strapi-sandbox.git
   cd strapi-sandbox
   ```

1. Copy the `.env.example` file to `.env`

   ```bash
   cp .env.example .env
   ```

   Note that `docker-compose.yml` defines some default environment variables. `API_TOKEN_SALT`, `TRANSFER_TOKEN_SALT`, etc.

1. Start the sandbox with Docker

   ```bash
   docker compose up -d
   ```

   This will start 2 instances for Postgres and Strapi.

   - `postgres` is the "production" database instance
   - `strapi` is the "production" Strapi instance _(available at port `1337` on the host)_
   - `postgres-staging` is the "staging" database instance
   - `strapi-staging` is the "staging" Strapi instance _(available at port `1338` on the host)_

1. Experiment with the Strapi CLI

   ```bash
   docker exec -it strapi-staging bash
   npm run strapi -- --help
   ```

1. Update the configuration and data on the "production" instance (https://localhost:1337)

1. Copy data from the "production" instance to the "staging" instance

   _NOTE: You must create an API Token and Transfer Token on the "production" Strapi instance before proceeding._

   ```bash
   docker exec -it strapi-staging bash
   npm run strapi -- transfer --help
   npm run strapi -- transfer --from=http://strapi:1337/admin --from-token=SECRET
   ```

1. Shutdown the sandbox

   ```bash
   docker compose down
   ```
