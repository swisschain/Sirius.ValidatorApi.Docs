# Blockchains

We distinguish *Blockchain*, *Protocol*, and *Network Type*.

*Blockchain* is not just Bitcoin or Ethereum, it's a particular network. For example Bitcoin MainNet, Ethereum Ropsten are blockchains, whilst Bitcoin and Ethereum are protocols which are used by these blockchains. Networks can be one of the types - `private`, `test`, `public`. So Blockchain has a protocol and network type.

*Blockchain* is an "instance" of the protocol - Ethereum Ropsten, Bitcoin Mainnet, Ethereum Mainnet.

Protocol is a set of rules on how blockchain nodes interact with each other - `Bitcoin`, `Ethereum`, `Ripple`.

Network type can be `private`, `test`, or `public`.

## Search blockchains

### Request

**Rest API:** `GET /api/blockchains`

### Query Parameters

name | type | description | example
---- | ---- | ----------- | -------
`id` | *optional*, *string* | Exact ID of the blockchain to search | bitcoin-private
`name` | *optional*, *string* | Text to search in the name of the blockchain | Bitcoin private network
`protocolCode` | *optional*, *string* | Exact ode of the protocol to search | bitcoin
`protocolName` | *optional*, *string* | Text to search in the name of the protocol | Bitcoin
`networkType` | *optional*, *[NetworkType](#networktype-enum)* | List of the network types to search. Repeat the parameter to specify multiple items | private
`order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | *optional*, *string* | Cursor to continue the search | stellar-test
`limit` | *optional*, *number* | Maximum number of items to return in the search results | 10

### Response

Paginated array of the blockchains:

name | type | description | example
---- | ---- | ----------- | -------
`id` | string | Unique identifier. | `bitcoin-private`
`tenantId` | *optional*, *string* | Id of the tenant that owns the blockchain | 100000
`name` | *string* | Name of the blockchain | `bitcoin-private`
`networkType` | *[NetworkType](#networktype-enum)* | Type of the network
`protocol` | *[BlockchainProtocol](#blockchainprotocol-object)* | Protocol of the blockchain.
`latestBlockNumber` | *number* | Number of the latest block | 1567432

> GET /api/blockchains 200 OK

```json
{
  "pagination": {
    "cursor": null,
    "count": 3,
    "order": "asc",
    "nextUrl": "/api/blockchains?Order=Asc&Cursor=stellar-test&Limit=3"
  },
  "items": [
    {
      "id": "bitcoin-private",
      "protocol": {
        "code": "bitcoin",
        "name": "Bitcoin",
        "startBlockNumber": 0,
        "requirements": {
          "publicKey": true
        },
        "capabilities": {
          "destinationTag": {
            "number": null,
            "text": null
          }
        },
        "doubleSpendingProtectionType": "coins"
      },
      "tenantId": null,
      "name": "Bitcoin private network",
      "networkType": "private",
      "latestBlockNumber": 5899
    },
    {
      "id": "ethereum-ropsten",
      "protocol": {
        "code": "ethereum",
        "name": "Ethereum",
        "startBlockNumber": 0,
        "requirements": {
          "publicKey": false
        },
        "capabilities": {
          "destinationTag": {
            "number": null,
            "text": null
          }
        },
        "doubleSpendingProtectionType": "nonce"
      },
      "tenantId": null,
      "name": "Ethereum test network",
      "networkType": "test",
      "latestBlockNumber": 8450781
    },
    {
      "id": "stellar-test",
      "protocol": {
        "code": "stellar",
        "name": "Stellar",
        "startBlockNumber": 1,
        "requirements": {
          "publicKey": false
        },
        "capabilities": {
          "destinationTag": {
            "number": {
              "min": 1,
              "max": 9223372036854776000,
              "name": "Memo ID"
            },
            "text": {
              "maxLength": 28,
              "name": "Memo text"
            }
          }
        },
        "doubleSpendingProtectionType": "nonce"
      },
      "tenantId": null,
      "name": "Stellar test network",
      "networkType": "test",
      "latestBlockNumber": 133093
    }
  ]
}
```

## Get latest block

### Request

**Rest API:** `GET /api/blockchains/{id}/latest-block`

### Parameters

name | type | description | example
---- | ---- | ----------- | -------
`id` | *string* | Exact ID of the blockchain to search | bitcoin-private

### Response

The latest block of the specified blockchain:

name | type | description | example
---- | ---- | ----------- | -------
`blockNumber` | *number* | Number of the latest block. | 5933

> GET /api/blockchains/{id}/latest-block 200 OK

```json
{
  "blockNumber": 5933
}
```