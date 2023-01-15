# fragrances-emporium

This project was generated with [`@vendure/create`](https://github.com/vendure-ecommerce/vendure/tree/master/packages/create).

Useful links:

- [Vendure docs](https://www.vendure.io/docs)
- [Vendure Slack community](https://join.slack.com/t/vendure-ecommerce/shared_invite/zt-1exzio25w-vjL5TYkyJZjK52d6jkOsIA)
- [Vendure on GitHub](https://github.com/vendure-ecommerce/vendure)
- [Vendure plugin template](https://github.com/vendure-ecommerce/plugin-template)

## Directory structure

* `/src` contains the source code of your Vendure server. All your custom code and plugins should reside here.
* `/static` contains static (non-code) files such as assets (e.g. uploaded images) and email templates.

## Development

```
yarn dev
```

will start the Vendure server and [worker](https://www.vendure.io/docs/developer-guide/vendure-worker/) processes from
the `src` directory.

## Build

```
yarn build
```

will compile the TypeScript sources into the `/dist` directory.

## Production

For production, there are many possibilities which depend on your operational requirements as well as your production
hosting environment.

### Running directly

You can run the built files directly with the `start` script:

```
yarn start
```

You could also consider using a process manager like [pm2](https://pm2.keymetrics.io/) to run and manage
the server & worker processes.

### Using Docker

We've included a sample [Dockerfile](./Dockerfile) which you can build with the following command:

```
docker build -t vendure .
```

and then run it with:
```
# Run the server
docker run -dp 3000:3000 -e "DB_HOST=host.docker.internal" --name vendure-server vendure npm run start:server

# Run the worker
docker run -dp 3000:3000 -e "DB_HOST=host.docker.internal" --name vendure-worker vendure npm run start:worker
```

### Docker compose

We've included a sample [docker-compose.yml](./docker-compose.yml) file which demonstrates how the server, worker, and
database may be orchestrated with Docker Compose.

## Plugins

In Vendure, your custom functionality will live in [plugins](https://www.vendure.io/docs/plugins/).
These should be located in the `./src/plugins` directory.

## Migrations

[Migrations](https://www.vendure.io/docs/developer-guide/migrations/) allow safe updates to the database schema. Migrations
will be required whenever you make changes to the `customFields` config or define new entities in a plugin.

The following npm scripts can be used to generate migrations:

```
yarn migration:generate [name]
```

The generated migration file will be found in the `./src/migrations/` directory, and should be committed to source control.
Next time you start the server, and outstanding migrations found in that directory will be run by the `runMigrations()`
function in the [index.ts file](./src/index.ts).

If, during initial development, you do not wish to manually generate a migration on each change to customFields etc, you
can set `dbConnectionOptions.synchronize` to `true`. This will cause the database schema to get automatically updated
on each start, removing the need for migration files. Note that this is **not** recommended once you have production
data that you cannot lose.

---

You can also run any pending migrations manually, without starting the server by running:

```
yarn migration:run
```

You can revert the most recently-applied migration with:

```
yarn migration:revert
```
