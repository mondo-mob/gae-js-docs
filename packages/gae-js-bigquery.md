---
layout: default
title: Big Query
parent: Packages
nav_order: 8
---

# GAE JS BIGQUERY

> **Replaced with [MondoKit](https://mondokit.dev)**: We are excited to announce the next generation of GAE JS. Our `ESM-only` set of libraries, migrated from GAE JS and rebranded as [MondoKit](https://mondokit.dev).
>
> You can see a [Migration guide from GAE JS](https://mondokit.dev/migration-from-gae-js) to help you move to the new libraries. Many changes are simple search/replace.
>
> We will continue to add minor fixes and patches to this library as required.
>
> Find  [MondoKit on GitHub](https://github.com/mondo-mob/mondokit).

Simplifies BigQuery client initialisation and common BigQuery operations.

## Installation

```sh
npm install @mondomob/gae-js-bigquery
```

## Quick Start

### Using BigQuery Provider
To have a global BigQuery instance available to your entire application, initialise the `bigQueryProvider`.
This will initialise the BigQuery client based on your current configuration. The instance can
then be recalled anywhere within your application as required.

```typescript
// On app startup
bigQueryProvider.init();

// Elsewhere in the app
const bigQuery = bigQueryProvider.get();
```

### Manual connect to BigQuery
To manage your own BigQuery instances, use `connectBigQuery` to connect to BigQuery based on your
current application configuration.

```typescript
class CustomBigQueryService {
  private readonly bigQuery: BigQuery;

  constructor() {
    this.bigQuery = connectBigQuery()
  }
}
```

### Edit configuration
No configuration is required if you are happy with the default conventions. 

The following options are available under the `bigQuery` namespace.

| Property     | Description                                                                         | Required |
|--------------|-------------------------------------------------------------------------------------|----------|
| projectId    | the BigQuery projectId to connect to. Will default to the application's project id. | N        |

e.g.
```json
{
  "bigQuery": {
    "projectId": "my-bigquery-project"
  }
}
```
