# Tableland Validator API Specification

![draft](https://img.shields.io/badge/status-draft-yellow.svg?style=flat-square)

Author(s): [@jsign](https://github.com/jsign), [@carsonfarmer](https://github.com/carsonfarmer), [@brunocalza](https://github.com/brunocalza)

## Synopsis

This general API specification is used to automatically generate clients and backend services; it should be used as the primary source of truth for Validator APIs. This specification is a living document, and as such, may be updated over time. Proposals for the addition of API features and fixes may be submitted by the Tableland community over time. These proposals will be evaluated for technical feasibility, utility to the community, and longer-term sustainability.

### Table of Contents

-   [Query the network](#query-the-network)
-   [Get transaction status](#get-transaction-status)
-   [Get table information](#get-table-information)
-   [Get health status](#get-health-status)
-   [Get version information](#get-version-information)
-   [Schemas](#schemas)

<!-- Generator: Widdershins v4.0.1 -->
<h1 id="tableland-validator-openapi-3-0">
Tableland Validator - OpenAPI 3.0 v1.0.0
</h1>

> Scroll down for code samples, example requests and responses. Select a
> language for code samples from the tabs above or the mobile navigation
> menu.

In Tableland, Validators are the execution unit/actors of the protocol.
They have the following responsibilities: - Listen to on-chain events to
materialize Tableland-compliant SQL queries in a database engine
(currently, SQLite by default). - Serve read-queries (e.g: SELECT \*
FROM foo\_69\_1) to the external world. - Serve state queries (e.g. list
tables, get receipts, etc) to the external world.

In the 1.0.0 release of the Tableland Validator API, we’ve switched to a
design first approach! You can now help us improve the API whether it’s
by making changes to the definition itself or to the code. That way,
with time, we can improve the API in general, and expose some of the new
features in OAS3.

Base URLs:

-   <a href="https://testnets.tableland.network/api/v1">https://testnets.tableland.network/api/v1</a>

-   <a href="http://localhost:8080/api/v1">http://localhost:8080/api/v1</a>

<a href="http://docs.tableland.xyz">Terms of service</a> Email:
<a href="mailto:carson@textile.io">core devs</a> License:
<a href="http://www.apache.org/licenses/LICENSE-2.0.html">Apache 2.0</a>

<h1 id="tableland-validator-openapi-3-0-query">
query
</h1>

Query the Tableland network

<a href="http://docs.tableland.xyz">Find out more about queries</a>

## Query the network

<a id="opIdqueryByStatement"></a>

> Code samples

``` shell
# You can also use wget
curl -X GET https://testnets.tableland.network/api/v1/query?statement=select%20%2A%20from%20healthbot_80001_1 \
  -H 'Accept: application/json'
```

`GET /query`

Returns the results of a SQL read query against the Tabeland network

<h3 id="query-the-network-parameters">
Parameters
</h3>

| Name      | In    | Type    | Required | Description                                                                                 |
|-----------|-------|---------|----------|---------------------------------------------------------------------------------------------|
| statement | query | string  | true     | The SQL read query statement                                                                |
| format    | query | string  | false    | The requested response format:                                                              |
| extract   | query | boolean | false    | Whether to extract the JSON object from the single property of the surrounding JSON object. |
| unwrap    | query | boolean | false    | Whether to unwrap the returned JSON objects from their surrounding array.                   |

#### Detailed descriptions

**format**: The requested response format: \* `objects` - Returns the
query results as a JSON array of JSON objects. \* `table` - Return the
query results as a JSON object with columns and rows properties.

#### Enumerated Values

| Parameter | Value   |
|-----------|---------|
| format    | objects |
| format    | table   |

> Example responses

> 200 Response

``` json
{}
```

<h3 id="query-the-network-responses">
Responses
</h3>

| Status | Meaning                                                            | Description                   | Schema |
|--------|--------------------------------------------------------------------|-------------------------------|--------|
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)            | Successful operation          | Inline |
| 400    | [Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)   | Invalid query/statement value | None   |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)     | Row Not Found                 | None   |
| 429    | [Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4) | Too Many Requests             | None   |

<h3 id="query-the-network-responseschema">
Response Schema
</h3>
<aside class="success">
This operation does not require authentication
</aside>
<h1 id="tableland-validator-openapi-3-0-receipt">
receipt
</h1>

Get information about transaction progress

<a href="http://docs.tableland.xyz">Find out more about receipts</a>

## Get transaction status

<a id="opIdreceiptByTransactionHash"></a>

> Code samples

``` shell
# You can also use wget
curl -X GET https://testnets.tableland.network/api/v1/receipt/{chainId}/{transactionHash} \
  -H 'Accept: application/json'
```

`GET /receipt/{chainId}/{transactionHash}`

Returns the status of a given transaction receipt by hash

<h3 id="get-transaction-status-parameters">
Parameters
</h3>

| Name            | In   | Type           | Required | Description                     |
|-----------------|------|----------------|----------|---------------------------------|
| chainId         | path | integer(int32) | true     | The parent chain to target      |
| transactionHash | path | string         | true     | The transaction hash to request |

> Example responses

> 200 Response

``` json
{
  "table_id": "1",
  "transaction_hash": "0x02f319429b8a7be1cbb492f0bfbf740d2472232a2edadde7df7c16c0b61aa78b",
  "block_number": 27055540,
  "chain_id": 80001,
  "error": "The query statement is invalid",
  "error_event_idx": 1
}
```

<h3 id="get-transaction-status-responses">
Responses
</h3>

| Status | Meaning                                                            | Description                                         | Schema                                          |
|--------|--------------------------------------------------------------------|-----------------------------------------------------|-------------------------------------------------|
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)            | successful operation                                | [TransactionReceipt](#schematransactionreceipt) |
| 400    | [Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)   | Invalid chain identifier or transaction hash format | None                                            |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)     | No transaction receipt found with the provided hash | None                                            |
| 429    | [Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4) | Too Many Requests                                   | None                                            |

<aside class="success">
This operation does not require authentication
</aside>
<h1 id="tableland-validator-openapi-3-0-tables">
tables
</h1>

Access to table information

<a href="http://docs.tableland.xyz">Find out more about tables</a>

## Get table information

<a id="opIdgetTableById"></a>

> Code samples

``` shell
# You can also use wget
curl -X GET https://testnets.tableland.network/api/v1/tables/{chainId}/{tableId} \
  -H 'Accept: application/json'
```

`GET /tables/{chainId}/{tableId}`

Returns information about a single table, including schema information

<h3 id="get-table-information-parameters">
Parameters
</h3>

| Name    | In   | Type           | Required | Description                |
|---------|------|----------------|----------|----------------------------|
| chainId | path | integer(int32) | true     | The parent chain to target |
| tableId | path | string         | true     | Table identifier           |

> Example responses

> 200 Response

``` json
{
  "name": "healthbot_5_1",
  "external_url": "https://testnet.tableland.network/tables/healthbot_5_1",
  "animation_url": "https://render.tableland.xyz/anim/?chain=1&id=1",
  "image": "https://render.tableland.xyz/healthbot_5_1",
  "attributes": {
    "display_type": "date",
    "trait_type": "created",
    "value": 1657113720
  },
  "schema": {
    "columns": [
      {
        "name": "id",
        "type": "integer",
        "constraints": [
          "NOT NULL",
          "PRIMARY KEY",
          "UNIQUE"
        ]
      }
    ],
    "table_constraints": [
      "PRIMARY KEY (id)"
    ]
  }
}
```

<h3 id="get-table-information-responses">
Responses
</h3>

| Status | Meaning                                                                    | Description                       | Schema                |
|--------|----------------------------------------------------------------------------|-----------------------------------|-----------------------|
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)                    | successful operation              | [Table](#schematable) |
| 400    | [Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)           | Invalid chain or table identifier | None                  |
| 404    | [Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)             | Table Not Found                   | None                  |
| 429    | [Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)         | Too Many Requests                 | None                  |
| 500    | [Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) | Internal Server Error             | None                  |

<aside class="success">
This operation does not require authentication
</aside>
<h1 id="tableland-validator-openapi-3-0-health">
health
</h1>

## Get health status

<a id="opIdhealth"></a>

> Code samples

``` shell
# You can also use wget
curl -X GET https://testnets.tableland.network/api/v1/health
```

`GET /health`

Returns OK if the validator considers itself healthy.

<h3 id="get-health-status-responses">
Responses
</h3>

| Status | Meaning                                                 | Description               | Schema |
|--------|---------------------------------------------------------|---------------------------|--------|
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | The validator is healthy. | None   |

<aside class="success">
This operation does not require authentication
</aside>
<h1 id="tableland-validator-openapi-3-0-version">
version
</h1>

## Get version information

<a id="opIdversion"></a>

> Code samples

``` shell
# You can also use wget
curl -X GET https://testnets.tableland.network/api/v1/version \
  -H 'Accept: application/json'
```

`GET /version`

Returns version information about the validator daemon.

> Example responses

> 200 Response

``` json
{
  "version": 0,
  "git_commit": "79688910d4689dcc0991a0d8eb9d988200586d8f",
  "git_branch": "foo/experimentalfeature",
  "git_state": "dirty",
  "git_summary": "v1.2.3_dirty",
  "build_date": "2022-11-29T16:28:04Z",
  "binary_version": "v1.0.1"
}
```

<h3 id="get-version-information-responses">
Responses
</h3>

| Status | Meaning                                                 | Description          | Schema                            |
|--------|---------------------------------------------------------|----------------------|-----------------------------------|
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | successful operation | [VersionInfo](#schemaversioninfo) |

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Table">
Table
</h2>
<!-- backwards compatibility -->

<a id="schematable"></a> <a id="schema_Table"></a>
<a id="tocStable"></a> <a id="tocstable"></a>

``` json
{
  "name": "healthbot_5_1",
  "external_url": "https://testnet.tableland.network/tables/healthbot_5_1",
  "animation_url": "https://render.tableland.xyz/anim/?chain=1&id=1",
  "image": "https://render.tableland.xyz/healthbot_5_1",
  "attributes": {
    "display_type": "date",
    "trait_type": "created",
    "value": 1657113720
  },
  "schema": {
    "columns": [
      {
        "name": "id",
        "type": "integer",
        "constraints": [
          "NOT NULL",
          "PRIMARY KEY",
          "UNIQUE"
        ]
      }
    ],
    "table_constraints": [
      "PRIMARY KEY (id)"
    ]
  }
}
```

### Properties

| Name            | Type       | Required | Restrictions | Description                       |
|-----------------|------------|----------|--------------|-----------------------------------|
| name            | string     | false    | none         | none                              |
| external\_url   | string     | false    | none         | none                              |
| animation\_url  | string     | false    | none         | none                              |
| image           | string     | false    | none         | none                              |
| attributes      | \[object\] | false    | none         | none                              |
| » display\_type | string     | false    | none         | The display type for marketplaces |
| » trait\_type   | string     | false    | none         | The trait type for marketplaces   |
| » value         | object     | false    | none         | The value of the property         |

oneOf

| Name           | Type   | Required | Restrictions | Description |
|----------------|--------|----------|--------------|-------------|
| »» *anonymous* | string | false    | none         | none        |

xor

| Name           | Type   | Required | Restrictions | Description |
|----------------|--------|----------|--------------|-------------|
| »» *anonymous* | number | false    | none         | none        |

xor

| Name           | Type    | Required | Restrictions | Description |
|----------------|---------|----------|--------------|-------------|
| »» *anonymous* | integer | false    | none         | none        |

xor

| Name           | Type    | Required | Restrictions | Description |
|----------------|---------|----------|--------------|-------------|
| »» *anonymous* | boolean | false    | none         | none        |

xor

| Name           | Type   | Required | Restrictions | Description |
|----------------|--------|----------|--------------|-------------|
| »» *anonymous* | object | false    | none         | none        |

continued

| Name   | Type                    | Required | Restrictions | Description |
|--------|-------------------------|----------|--------------|-------------|
| schema | [Schema](#schemaschema) | false    | none         | none        |

<h2 id="tocS_TransactionReceipt">
TransactionReceipt
</h2>
<!-- backwards compatibility -->

<a id="schematransactionreceipt"></a>
<a id="schema_TransactionReceipt"></a>
<a id="tocStransactionreceipt"></a> <a id="tocstransactionreceipt"></a>

``` json
{
  "table_id": "1",
  "transaction_hash": "0x02f319429b8a7be1cbb492f0bfbf740d2472232a2edadde7df7c16c0b61aa78b",
  "block_number": 27055540,
  "chain_id": 80001,
  "error": "The query statement is invalid",
  "error_event_idx": 1
}
```

### Properties

| Name              | Type           | Required | Restrictions | Description |
|-------------------|----------------|----------|--------------|-------------|
| table\_id         | string         | false    | none         | none        |
| transaction\_hash | string         | false    | none         | none        |
| block\_number     | integer(int64) | false    | none         | none        |
| chain\_id         | integer(int32) | false    | none         | none        |
| error             | string         | false    | none         | none        |
| error\_event\_idx | integer(int32) | false    | none         | none        |

<h2 id="tocS_Schema">
Schema
</h2>
<!-- backwards compatibility -->

<a id="schemaschema"></a> <a id="schema_Schema"></a>
<a id="tocSschema"></a> <a id="tocsschema"></a>

``` json
{
  "columns": [
    {
      "name": "id",
      "type": "integer",
      "constraints": [
        "NOT NULL",
        "PRIMARY KEY",
        "UNIQUE"
      ]
    }
  ],
  "table_constraints": [
    "PRIMARY KEY (id)"
  ]
}
```

### Properties

| Name               | Type                        | Required | Restrictions | Description |
|--------------------|-----------------------------|----------|--------------|-------------|
| columns            | \[[Column](#schemacolumn)\] | false    | none         | none        |
| table\_constraints | \[string\]                  | false    | none         | none        |

<h2 id="tocS_Column">
Column
</h2>
<!-- backwards compatibility -->

<a id="schemacolumn"></a> <a id="schema_Column"></a>
<a id="tocScolumn"></a> <a id="tocscolumn"></a>

``` json
{
  "name": "id",
  "type": "integer",
  "constraints": [
    "NOT NULL",
    "PRIMARY KEY",
    "UNIQUE"
  ]
}
```

### Properties

| Name        | Type       | Required | Restrictions | Description |
|-------------|------------|----------|--------------|-------------|
| name        | string     | false    | none         | none        |
| type        | string     | false    | none         | none        |
| constraints | \[string\] | false    | none         | none        |

<h2 id="tocS_VersionInfo">
VersionInfo
</h2>
<!-- backwards compatibility -->

<a id="schemaversioninfo"></a> <a id="schema_VersionInfo"></a>
<a id="tocSversioninfo"></a> <a id="tocsversioninfo"></a>

``` json
{
  "version": 0,
  "git_commit": "79688910d4689dcc0991a0d8eb9d988200586d8f",
  "git_branch": "foo/experimentalfeature",
  "git_state": "dirty",
  "git_summary": "v1.2.3_dirty",
  "build_date": "2022-11-29T16:28:04Z",
  "binary_version": "v1.0.1"
}
```

### Properties

| Name            | Type           | Required | Restrictions | Description |
|-----------------|----------------|----------|--------------|-------------|
| version         | integer(int32) | false    | none         | none        |
| git\_commit     | string         | false    | none         | none        |
| git\_branch     | string         | false    | none         | none        |
| git\_state      | string         | false    | none         | none        |
| git\_summary    | string         | false    | none         | none        |
| build\_date     | string         | false    | none         | none        |
| binary\_version | string         | false    | none         | none        |
