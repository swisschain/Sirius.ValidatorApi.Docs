# Approval requests

Sirius provides the set of crypto assets available to use. You can get notifications about a deposit or initiate a withdrawal in any asset presented in this set. Each asset has a symbol, accuracy, optional address, ID of the blockchain containing this asset and unique ID. It's possible that in the different blockchains assets with the same symbol and/or address are existed, so you have to rely only on the asset ID if you need to deterministically identify it. In fact asset ID corresponds to its blockchain ID + symbol + address. If you've received a deposit of an asset which is not supported by Sirius yet, you can add this particular asset to your personal list by specifying all needed asset parameters.

## Search assets

### Request

**Rest API:** `GET /api/assets`

### Parameters

### Query Parameters

name | type | description | example
---- | ---- | ----------- | -------
`id` | *optional*, *number* | ID of the asset to search | 100553
`blockchainId` | *optional*, *string* | Exact blockchain id to search | bitcoin-private
`symbol` | *optional*, *string* | Text to search in the asset symbol | BTC
`address` | *optional*, *string* | Exact address to search | 0xC1701AbD559FC263829CA3917d03045F95b5224A
`accuracy` | *optional*, *number* | Exact accuracy to search | 8
`order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | *optional*, *string* | Cursor to continue the search | stellar-test
`limit` | *optional*, *number* | Maximum number of items to return in the search results | 10

### Response

Paginated array of the assets:

name | type | description | example
---- | ---- | ----------- | -------
`id` | *number* | ID of the asset to search | 100003
`blockchainId` | *string* | Exact blockchain id to search | bitcoin-private
`symbol` | *string* | Text to search in the asset symbol | BTC
`address` | *optional*, *string* | Exact address to search | 0xC1701AbD559FC263829CA3917d03045F95b5224A
`accuracy` | *number* | Exact accuracy to search | 8

> GET /api/assets 200 OK

```json
{
  "pagination": {
    "cursor": null,
    "count": 3,
    "order": "asc",
    "nextUrl": "/api/assets?Symbol=BTC&Order=Asc&Cursor=100110&Limit=3"
  },
  "items": [
    {
      "id": 100003,
      "blockchainId": "ethereum-ropsten",
      "symbol": "WBTC",
      "address": "0x3DFf0dCE5fc4B367Ec91d31DE3837cf3840c8284",
      "accuracy": 8
    },
    {
      "id": 100085,
      "blockchainId": "ethereum-ropsten",
      "symbol": "sBTC",
      "address": "0xC1701AbD559FC263829CA3917d03045F95b5224A",
      "accuracy": 18
    },
    {
      "id": 100110,
      "blockchainId": "ethereum-ropsten",
      "symbol": "TBTC",
      "address": "0xa609f2c9C5B7873F353C15D4ef6e151D14db69CC",
      "accuracy": 18
    }
  ]
}
```