---
layout: default
title: Home
nav_order: 1
description: "Simplify building NodeJS applications on Google App Engine (GAE)"
permalink: /
---

# GAE JS

Simplify building NodeJS applications on Google App Engine (GAE)

## Installation

Install the core component:
```sh
npm install @mondomob/gae-js-core
```

Then depending on your use case install the components you want to use. e.g.

```sh
npm install @mondomob/gae-js-firestore
npm install @mondomob/gae-js-firebase-auth
```

## Usage
Here's an example Express app configuration that uses the core library as well as both Firestore and Firebase Auth.

```
// Create a request aware logger
const logger = createLogger("gae-js-demo");

// Create your express app as normal
const app = express();

// Add the core gae-js logging and async storage middlewares
app.use(gaeJsApp);

// Add firestore support (with dataloader for graphql support)
firestoreProvider.init();
app.use(firestoreLoader());

// Add firebase auth support
const firebaseAdmin = admin.initializeApp({ projectId: <my-gcp-project-id> });
app.use(verifyFirebaseUser(firebaseAdmin));

// Add handlers as required
app.get(
  "/demo-items",
  requiresUser(),                   // <-- Util middleware that enforces a valid user
  asyncMiddleware(async (req, res) => { // <-- Util middleware that lets you write async handlers
    const repository = new FirestoreRepository<DemoItem>("demo-items");
    logger.info("listing demo-items from firestore");
    const list = await repository.query();
    res.send(`Hello firestore ${JSON.stringify(list)}`);
  })
);

app.use((err, req, res, next) => {
  // your error handling logic goes here
  logger.error("Error", err);
  res.status(500).send("Bad stuff happened")
});
```

## Background

This library is mostly a deconstructed version of what is offered by
[gae-node-nestjs](https://github.com/mondo-mob/gae-node-nestjs) and
[generator-gae-node-nestjs](https://www.npmjs.com/package/@mondomob/generator-gae-node-nestjs) libraries.

You can do a lot with these libs but they can be difficult to customise. For example the data layer
is heavily tied to using Firestore in Datastore Mode and assumes you want to use graphql. It's not
a great fit if you only want to build a simple API with Firestore Native db.

So the intention with this library was to offer a similar feature set but in a minimal way.
i.e. almost everything is optional.

## Components

### gae-js-core ([documentation](packages/gae-js-core.md))

#### Async Local Storage Support

This library relies on Async Local Storage for passing data around. Yes it's still an Experimental NodeJS
API but it's really useful and the general push is to mark it as stable soon (https://github.com/nodejs/node/issues/35286).

#### Logging
Create request aware Bunyan loggers for Cloud Logging.
All of your logs from a single request will be correlated together in Cloud Logging.

#### Configuration
Extendable, typed and environment aware configuration loader

#### Configuration Secrets
Seamlessly load secrets stored in "Google Cloud Secret Manager"

#### Serving static resources with etags
Serve static assets with strong etags to workaround GAE build wiping out your file timestamps

#### Authentication/Authorization
Middleware to protect your routes to authenticated users or specific user roles

#### Search
Framework for adding search capability to your data layer

#### Other stuff
A few other (hopefully) useful things to help you along the way and stop reinventing the wheel

### gae-js-bigquery ([documentation](packages/gae-js-bigquery.md))
#### Use BigQuery in your app
Simplifies client initialisation and common BQ tasks

### gae-js-datastore ([documentation](packages/gae-js-datastore.md))

#### Use Cloud Datastore (or Firestore in Datastore mode)
Access your collections through typed repositories, backed by a DataLoader implementation to support GraphQL.

#### Simple transaction support
Use annotations on your methods to make them transactional

### gae-js-datastore-backups ([documentation](packages/gae-js-datastore-backups.md))
#### Automate Datastore Exports
Run/schedule full or partial exports of Datastore into Google Cloud Storage
#### Import into BigQuery
Schedule Extract and Load jobs to export from Datastore into BigQuery

### gae-js-firebase-auth ([documentation](packages/gae-js-firebase-auth.md))
#### Use Firebase Auth to authenticate your users
Middleware to verify Firebase Auth tokens and set user into the request

### gae-js-firestore ([documentation](packages/gae-js-firestore.md))

#### Use Firestore in Native mode
Access your collections through typed repositories, backed by a DataLoader implementation to support GraphQL.

#### Simple transaction support
Use annotations on your methods to make them transactional

### gae-js-firestore-backups ([documentation](packages/gae-js-firestore-backups.md))
#### Automate Firestore Exports
Run/schedule full or partial exports of Firestore into Google Cloud Storage
#### Import into BigQuery
Schedule Extract and Load jobs to export from Firestore into BigQuery

### gae-js-gae-search ([documentation](packages/gae-js-gae-search.md))
#### Search service implementation for GAE Search API
Use GAE Search API to index and search your repository data

### gae-js-google-auth ([documentation](./packages/gae-js-google-auth.md))
#### Google Auth utilities
Utilities extending on [Google Auth Library](https://github.com/googleapis/google-auth-library-nodejs#readme), such as middleware to validate Google JWT.

### gae-js-migrations ([documentation](./packages/gae-js-migrations.md))
#### Run migrations
Bootstrap migrations to be run when server starts or create an endpoint to trigger them. Status of migrations and mutex lock is managed with Firestore.

### gae-js-storage ([documentation](./packages/gae-js-storage.md))
#### Use Cloud Storage in your app
Simplifies client initialisation and common storage tasks

### gae-js-tasks ([documentation](./packages/gae-js-tasks.md))
#### Use Cloud Tasks in your app
Simplifies client initialisation and common task operations

