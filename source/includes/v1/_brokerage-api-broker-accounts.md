# Broker accounts

Broker account is the multi-currency (multi-blockchain) account which can contain unlimited number of personalized accounts within it. Broker account aggregates funds deposited to the personalized accounts and it provides funds for the withdrawals. The private keys of the broker accounts are stored in the vault associated with the broker account. The vault can't be changed once broker account is created.

You can choose which particular blockchain should be enabled for the given broker account. You can give a name to the broker account which make sense for you. It's convenient to have separate broker account for each business case. It's also reasonable to keep funds on the different brokerage accounts because of security reasons. For instance you can have a "Hot" broker account for operational work, "Cold" broker account to keep reserve funds which can be used by the limited employees group of your company, and "Frozen" broker account which can be used only by the owners of the business. Then you can setup a process to settle funds between these broker accounts on a periodical basis or on a demand.

Under the hood a broker account manages set of blockchain addresses. It has at least one address for each blockchain enabled for it. Depending on a particularities of each blockchain separate address or unique destination tag is created for each account within the broker account. Deposits received on an address of an account automatically moved to the address of the broker account. For some assets it's needed to pay fees in a different asset to move funds from an account to the broker account. For this Sirius provides "fees assets" on the account address by moving them from the broker account address.

Broker account structure:

<img src="https://github.com/swisschain/Sirius.Api.Docs/raw/master/source/images/broker-account-structure.png" alt="Broker account structure lifecycle"/>

## Create a broker account

### Request

**Rest API:** `POST /api/broker-accounts`

**gRPC API:** `swisschain.sirius.api.brokerAccounts.BrokerAccounts.Create`

```json
POST /api/broker-accounts

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
    "amlConnectionIds": [ 600000012 ],
    "blockchainIds": [ "bitcoin-test", "ethereum-ropsten"],
    "custodyId": 404000009,
    "name": "Broker Account Name"
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.Create

> Requets: (application/grpc)

message BrokerAccountCreateRequest {
  string request_id = 1;                    // 1a5c0b3d15494ec8a390fd3b22d757d6
  string name = 2;                          // "Broker Account Name"
  int64 custody_id = 3;                     // 404000009
  repeated string blockchain_ids = 4;       // [ "bitcoin-test", "ethereum-ropsten"] Array of blockchain ids
  repeated int64 aml_connection_ids = 5;    // [ 600000012 ] Array of aml connections ids
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`name` | `name` | *string* | *body* | Name of the created Broker Account
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`blockchainIds` | `blockchain_ids` | *Array of strings* | *body* | Ids of the blockchains with which broker Account should work
`amlConnectionIds` | `aml_connection_ids` | *Array of strings* | *body* | Ids of the AML connections to enable for deposit/withdrawal checks

### Response

```json
POST /api/broker-accounts 

> Response: 200 (application/json) - success response

{
    "name":"Broker Account Name",
    "id":100000115,
    "state":"creating",
    "accountCount":0,
    "blockchainsCount":2,"
    createdAt":"2021-03-30T09:02:44.7726151+00:00",
    "updatedAt":"2021-03-30T09:02:44.7726151+00:00",
    "custodyId":404000009,
    "custodyName":"NewSharedHSM",
    "blockchainIds":["bitcoin-test","ethereum-ropsten"],
    "amlConnectionIds":[600000012]
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.Create

> Response: (application/grpc) - success response

message BrokerAccountCreateResponse {
    oneof result {
      BrokerAccountCreateResponseBody body = 1;                 // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2; // Object
    } 
}

message BrokerAccountCreateResponseBody {
  BrokerAccountResponse broker_account = 1;                     // Object
}

message BrokerAccountResponse {
  string name = 1;                                              // "Broker Account Name"
  int64 id = 2;                                                 // 100000115
  BrokerAccountState state = 3;                                 // BrokerAccountState.Creating
  int64 accounts_count = 4;                                     // 0
  int64 blockchains_count = 5;                                  // 2
  google.protobuf.Timestamp created_at = 6;                     // "2021-03-30T09:02:44.7726151+00:00"
  google.protobuf.Timestamp updated_at = 7;                     // "2021-03-30T09:02:44.7726151+00:00"
  int64 custody_id = 8;                                         // 404000009
  string custody_name = 9;                                      // "NewSharedHSM"
  repeated string blockchain_ids = 10;                          // ["bitcoin-test","ethereum-ropsten"]
  repeated int64 aml_connection_ids = 11;                       // [600000012]
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account
`name` | `name` | *string* | *body* | Account reference id
`accountsCount` | `accounts_count` | *number* | Number of accounts that are attached to the broker account
`blockchainsCount` | `blockchains_count` | *number* | Number of already created broker account details
`state` | `state` | *[BrokerAccountState](#brokeraccountstate-enum)* | Status of the broker account
`createdAt` | `created_at` | *timestamp* | Date of the broker account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest broker account update
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchain_ids` | `blockchain_ids` | *Array of string* | *body* | Blockchains ids that are connected to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of numbers* | *body* | AML Connections that are enabled for the broker account


## Update the broker account

### Request

**Rest API:** `PUT /api/broker-accounts`

**gRPC API:** `swisschain.sirius.api.brokerAccounts.BrokerAccounts.Update`

```json
PUT /api/broker-accounts

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
    "amlConnectionIds": [600000007, 600000012],
    "blockchainIds": ["stellar-test"],
    "brokerAccountId": 100000116
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.Update

> Requets: (application/grpc)

message BrokerAccountUpdateRequest {
  string request_id = 1;                    // 1a5c0b3d15494ec8a390fd3b22d757d6
  int64 broker_account_id = 2;              // 100000116 
  repeated string blockchain_ids = 3;       // ["stellar-test"] only new ids could be added
  repeated int64 aml_connections_ids = 4;   // [600000007, 600000012] already existing ids could be removed
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`brokerAccountId` | `broker_account_id` | *number* | *body* | ID of the broker account
`blockchainIds` | `blockchain_ids` | *Array of strings* | *body* | Ids of the blockchains to add to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of strings* | *body* | Ids of the AML connections to enable for deposit/withdrawal checks

### Response

```json
PUT /api/broker-accounts 

> Response: 200 (application/json) - success response

{
    "name":"Broker Account Docs",
    "id":100000116,
    "state": "updating",
    "accountCount":0,
    "blockchainsCount":3,
    "createdAt":"2021-03-30T09:49:47.298466+00:00",
    "updatedAt":"2021-03-30T09:51:54.3471023+00:00",
    "custodyId":404000008,
    "custodyName": "SharedHSM_MAIN",
    "blockchainIds": ["bitcoin-test","ethereum-ropsten","stellar-test"],
    "amlConnectionIds":[600000007,600000012]
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.Update

> Response: (application/grpc) - success response

message BrokerAccountUpdateResponse {
  oneof result {
    BrokerAccountUpdateActionResponseBody body = 1;             // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;   // Object
  } 
}

message BrokerAccountUpdateActionResponseBody {
  BrokerAccountResponse broker_account = 1;                     // Object
}

message BrokerAccountResponse {
  string name = 1;                                              // "Broker Account Name"
  int64 id = 2;                                                 // 100000115
  BrokerAccountState state = 3;                                 // BrokerAccountState.Creating
  int64 accounts_count = 4;                                     // 0
  int64 blockchains_count = 5;                                  // 2
  google.protobuf.Timestamp created_at = 6;                     // "2021-03-30T09:02:44.7726151+00:00"
  google.protobuf.Timestamp updated_at = 7;                     // "2021-03-30T09:02:44.7726151+00:00"
  int64 custody_id = 8;                                         // 404000009
  string custody_name = 9;                                      // "NewSharedHSM"
  repeated string blockchain_ids = 10;                          // ["bitcoin-test","ethereum-ropsten"]
  repeated int64 aml_connection_ids = 11;                       // [600000012]
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account
`name` | `name` | *string* | *body* | Broker account name
`accountsCount` | `accounts_count` | *number* | Number of accounts that are attached to the broker account
`blockchainsCount` | `blockchains_count` | *number* | Number of already created broker account details
`state` | `state` | *[BrokerAccountState](#brokeraccountstate-enum)* | Status of the broker account
`createdAt` | `created_at` | *timestamp* | Date of the broker account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest broker account update
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchain_ids` | `blockchain_ids` | *Array of string* | *body* | Blockchains ids that are connected to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of numbers* | *body* | AML Connections that are enabled for the broker account

## Searches broker accounts

### Request

**Rest API:** `GET /api/broker-accounts`

**gRPC API:** `swisschain.sirius.api.brokerAccounts.BrokerAccounts.Search`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`id` | `id` | *optional*, *number* | Exact broker account ID to search | 100000113
`name` | `name` | *optional*, *string* | Name of the broker account  | Broker account name
`state` | `state` | *optional*, *Array of [BrokerAccountState](#brokeraccountstate-enum)* | State of the broker account | creating
- | `custody_id` | *optional*, *number* | Find broker accounts with specified Custody ID | 200000000
`order` | `pagination.order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.Search

> Requets: (application/grpc)

message BrokerAccountSearchRequest {
  google.protobuf.Int64Value id = 1;
  google.protobuf.StringValue name = 2;
  google.protobuf.Int64Value custody_id = 3;
  repeated BrokerAccountState state = 4;
  swisschain.sirius.api.common.PaginationInt64 pagination = 5;
}
```

### Response

```json
GET /api/broker-accounts

> Response: 200 (application/json) - success response

{
    "pagination":
    {
        "cursor":null,
        "count":10,
        "order":"asc",
        "nextUrl":"/api/broker-accounts?Order=Asc&Cursor=100000012&Limit=10"
    },
    "items":[
        {
            "name":"first",
            "id":100000000,
            "state":"creating",
            "accountCount":1,
            "blockchainsCount":5,
            "createdAt":"2020-09-08T13:10:24.949289+00:00",
            "updatedAt":"2020-09-08T13:10:24.949289+00:00",
            "custodyId":100000,
            "custodyName":"Shared1",
            "blockchainIds":["ethereum-ropsten","bitcoin-test","stellar-test","bitcoin-private","litecoin-private"],
            "amlConnectionIds":[]
        }, ...]
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.Search

> Response: (application/grpc) - success response

message BrokerAccountSearchResponse {
  oneof result {
    BrokerAccountSearchResponseBody body = 1;                   // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;   // Object
  } 
}

message BrokerAccountSearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated BrokerAccountResponse items = 2;                             // Object
}

message BrokerAccountResponse {
  string name = 1;                                              // "Broker Account Name"
  int64 id = 2;                                                 // 100000115
  BrokerAccountState state = 3;                                 // BrokerAccountState.Creating
  int64 accounts_count = 4;                                     // 0
  int64 blockchains_count = 5;                                  // 2
  google.protobuf.Timestamp created_at = 6;                     // "2021-03-30T09:02:44.7726151+00:00"
  google.protobuf.Timestamp updated_at = 7;                     // "2021-03-30T09:02:44.7726151+00:00"
  int64 custody_id = 8;                                         // 404000009
  string custody_name = 9;                                      // "NewSharedHSM"
  repeated string blockchain_ids = 10;                          // ["bitcoin-test","ethereum-ropsten"]
  repeated int64 aml_connection_ids = 11;                       // [600000012]
}
```

Paginated response of the broker accounts:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account
`name` | `name` | *string* | *body* | Broker account name
`accountsCount` | `accounts_count` | *number* | Number of accounts that are attached to the broker account
`blockchainsCount` | `blockchains_count` | *number* | Number of already created broker account details
`state` | `state` | *[BrokerAccountState](#brokeraccountstate-enum)* | Status of the broker account
`createdAt` | `created_at` | *timestamp* | Date of the broker account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest broker account update
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchain_ids` | `blockchain_ids` | *Array of string* | *body* | Blockchains ids that are connected to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of numbers* | *body* | AML Connections that are enabled for the broker account

## Searches the broker account balances

### Request

**Rest API:** `GET /api/broker-accounts/{brokerAccountId}/balances`

**gRPC API:** `swisschain.sirius.api.brokerAccounts.BrokerAccounts.GetBalances`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`brokerAccountId` | `broker_account_id` | *number* | Find balances for specified broker account ID | 200000000
`assetId` | `asset_id` | *optional*, *number* | Show balance for specified asset ID | 100000113
`order` | `pagination.order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.GetBalances

> Requets: (application/grpc)

message BrokerAccountGetBalancesRequest {
  int64 broker_account_id = 1;                                  // 200000000
  google.protobuf.Int64Value assetId = 2;                       // 100000113
  swisschain.sirius.api.common.PaginationInt64 pagination = 3;  // Object
}
```

### Response

```json
GET /api/broker-accounts/{brokerAccountId}/balances

> Response: 200 (application/json) - success response

{
    "pagination":
    {
        "cursor":null,
        "count":3,
        "order":"asc",
        "nextUrl":null
    },
    "items":[
        {
            "id":102000850,
            "brokerAccountId":100000113,
            "assetId":100000,
            "ownedBalance":0.17770416,
            "availableBalance":0.17770416,
            "pendingBalance":0.0,
            "reservedBalance":0.0,
            "createdAt":"2021-03-11T13:46:09.615581+00:00",
            "updatedAt":"2021-03-19T08:53:50.020425+00:00",
            "assetSymbol":"BTC",
            "assetAddress":null,
            "blockchainName":"Bitcoin private network"
        }
        , ...]
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.GetBalances

> Response: (application/grpc) - success response

message BrokerAccountGetBalancesResponse {
  oneof result {
    BrokerAccountGetBalancesResponseBody body = 1;                      // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;           // Object
  } 
}

message BrokerAccountGetBalancesResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated BrokerAccountBalancesResponse items = 2;                     // Paginated balances
}

message BrokerAccountBalancesResponse
{
  int64 id = 1;                                                         // 102000850
  int64 broker_account_id = 2;                                          // 100000113
  int64 asset_id = 3;                                                   // 100000
  swisschain.sirius.api.common.BigDecimal owned_balance = 4;            // 0.17770416
  swisschain.sirius.api.common.BigDecimal available_balance = 5;        // 0.17770416
  swisschain.sirius.api.common.BigDecimal pending_balance = 6;          // 0
  swisschain.sirius.api.common.BigDecimal reserved_balance = 7;         // 0
  google.protobuf.Timestamp created_at = 8;                             // "2021-03-11T13:46:09.615581+00:00"
  google.protobuf.Timestamp updated_at = 9;                             // "2021-03-19T08:53:50.020425+00:00"
  string asset_symbol = 10;                                             // "BTC"
  google.protobuf.StringValue asset_address = 11;                       // null
  string blockchain_name = 12;                                          // "Bitcoin private network"
}
```

Paginated response of the broker account balances:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account balance
`brokerAccountId` | `broker_account_id` | *number* | ID of the broker account
`assetId` | `asset_id` | *number* | ID of the asset
`ownedBalance` | `owned_balance` | *decimal* | Owned balance. (see balance diagram)
`availableBalance` | `available_balance` | *decimal* | Available balance. (see balance diagram)
`pendingBalance` | `pending_balance` | *decimal* | Pending balance. (see balance diagram)
`reservedBalance` | `reserved_balance` | *decimal* | Reserved balance. (see balance diagram)
`createdAt` | `created_at` | *timestamp* | Date of the broker account balance creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest broker account balance update
`assetSymbol` | `asset_symbol` | *string* | Symbol of the asset
`assetAddress` | `asset_address` | *string* | Asset's address on the blockchain(e.x:for tokens)
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchain_name` | `blockchain_name` | *string* | *body* | Blockchains name

## Searches the broker account details

### Request

**Rest API:** `GET /api/broker-accounts/{brokerAccountId}/details`

**gRPC API:** `swisschain.sirius.api.brokerAccounts.BrokerAccounts.SearchDetails`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`brokerAccountId` | `broker_account_id` | *number* | Find details for specified broker account ID | 200000000
`id` | `id` | *optional*, *number* | Find details for specified broker account ID | 200000001
`assetId` | `asset_id` | *optional*, *number* | Show details for specified asset ID | 100000
`blockchainId` | `blockchain_id` | *optional*, *number* | Show details for specified blockchain ID | ethereum-ropsten
`address` | `address` | *optional*, *number* | Show details for specified address | 0x678401cf8200967a3998d1480512b3f9e79f9447
`order` | `pagination.order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.SearchDetails

> Requets: (application/grpc)

message BrokerAccountDetailsSearchRequest {
  int64 broker_account_id = 1;                                  // 200000000
  google.protobuf.Int64Value id = 2;                            // 200000001
  google.protobuf.Int64Value assetId = 3;                       // 100000
  google.protobuf.StringValue blockchainId = 4;                 // "ethereum-ropsten"
  google.protobuf.StringValue address = 5;                      // "0x678401cf8200967a3998d1480512b3f9e79f9447"
  swisschain.sirius.api.common.PaginationInt64 pagination = 6;  // Pagination object
}
```

### Response

```json
GET /api/broker-accounts/{brokerAccountId}/details

> Response: 200 (application/json) - success response

{
    "pagination": {
        "cursor":null,
        "count":2,
        "order":"asc",
        "nextUrl":null
        },
        "items":[
            {
                "id":101000254,
                "brokerAccountId":100000113,
                "createdAt":"2021-03-10T12:27:44.666695+00:00",
                "address":"0x02d45A855dB2dD98899cdA9a58c16E8f4A1faA96",
                "blockchainId":"ethereum-ropsten"
            }, ...]
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.SearchDetails

> Response: (application/grpc) - success response

message BrokerAccountDetailsSearchResponse {
  oneof result {
    BrokerAccountDetailsSearchResponseBody body = 1;                    // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;           // Object
  } 
}

message BrokerAccountDetailsSearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated BrokerAccountDetailsResponse items = 2;                      // Object
}

message BrokerAccountDetailsResponse {
  int64 id = 1;                                                         // 101000254
  int64 broker_account_id = 2;                                          // 100000113
  google.protobuf.Timestamp created_at = 3;                             // "2021-03-10T12:27:44.666695+00:00"
  string address = 4;                                                   // "0x02d45A855dB2dD98899cdA9a58c16E8f4A1faA96"
  string blockchain_id = 5;                                             // "ethereum-ropsten"
}
```

Paginated response of the broker account details:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account balance
`brokerAccountId` | `broker_account_id` | *number* | ID of the broker account
`assetId` | `asset_id` | *number* | ID of the asset
`createdAt` | `created_at` | *timestamp* | Date of the broker account balance creation
`address` | `address` | *string* | Asset's address on the blockchain(e.x:for tokens)
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchainId` | `blockchain_id` | *string* | *body* | Blockchain ID

## Gets the broker account details by the asset id

### Request

**Rest API:** `GET /api/broker-accounts/{brokerAccountId}/details/by-asset-id/{assetId}`

**gRPC API:** `swisschain.sirius.api.brokerAccounts.BrokerAccounts.BrokerAccounts.GetDetails`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`brokerAccountId` | `broker_account_id` | *number* | Find details for specified broker account ID | 200000000
`assetId` | `asset_id` | *number* | Show details for specified asset ID | 100000


```protobuf
swisschain.sirius.api.brokerAccount.BrokerAccounts.GetDetails

> Requets: (application/grpc)

message BrokerAccountGetDetailsRequest {
  int64 brokerAccountId = 1;                        
  int64 assetId = 2;
}
```

### Response

```json
GET /api/broker-accounts/{brokerAccountId}/details/by-asset-id/{assetId}

> Response: 200 (application/json) - success response

{
    "id":101000254,
    "brokerAccountId":100000113,
    "createdAt":"2021-03-10T12:27:44.666695+00:00",
    "address":"0x02d45A855dB2dD98899cdA9a58c16E8f4A1faA96",
    "blockchainId":"ethereum-ropsten"
}
```

```protobuf
swisschain.sirius.api.brokerAccounts.BrokerAccounts.GetDetails

> Response: (application/grpc) - success response

message BrokerAccountGetDetailsResponse {
  oneof result {
    BrokerAccountGetDetailsResponseBody body = 1;                       // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;           // Object
  } 
}

message BrokerAccountGetDetailsResponseBody {
  BrokerAccountDetailsResponse broker_account_details = 1;              // Object
}

message BrokerAccountDetailsResponse {
  int64 id = 1;                                                         // 101000254
  int64 broker_account_id = 2;                                          // 100000113
  google.protobuf.Timestamp created_at = 3;                             // "2021-03-10T12:27:44.666695+00:00"
  string address = 4;                                                   // "0x02d45A855dB2dD98899cdA9a58c16E8f4A1faA96"
  string blockchain_id = 5;                                             // "ethereum-ropsten"
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account balance
`brokerAccountId` | `broker_account_id` | *number* | ID of the broker account
`assetId` | `asset_id` | *number* | ID of the asset
`createdAt` | `created_at` | *timestamp* | Date of the broker account balance creation
`address` | `address` | *string* | Asset's address on the blockchain(e.x:for tokens)
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchainId` | `blockchain_id` | *string* | *body* | Blockchain ID

## Search for broker account updates

### Request

**gRPC API:** `swisschain.sirius.api.brokerAccounts.BrokerAccounts.GetUpdates`

### Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
- | `custody_id` | *optional*, *number* | Search for broker accounts with this custody ID | 100000113
- | `broker_account_id` | *optional*, *number* | Exact broker account ID to search details for | 103000113
- | `name` | *optional*, *string* | Broker account name | user reference
- | `state` | *optional*, *Array of [BrokerAccountState](#brokeraccountstate-enum)* | State of the account | creating
- | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110


```protobuf
swisschain.sirius.api.accounts.BrokerAccounts.GetUpdates

> Requets: (application/grpc)

message BrokerAccountUpdateSearchRequest {
  google.protobuf.Int64Value broker_account_id = 1;
  google.protobuf.StringValue name = 2;
  google.protobuf.Int64Value custody_id = 3;
  repeated BrokerAccountState state = 4;
  google.protobuf.Int64Value cursor = 5;
}
```

### Response

```protobuf
swisschain.sirius.api.accounts.BrokerAccounts.GetUpdates

> Response: (application/grpc) - success response

message BrokerAccountUpdateArrayResponse{
  repeated BrokerAccountUpdateItemResponse items = 1;
}

message BrokerAccountUpdateItemResponse {
  string name = 1;
  int64 broker_account_id = 2;
  BrokerAccountState state = 3;
  google.protobuf.Timestamp created_at = 4;
  google.protobuf.Timestamp updated_at = 5;
  int64 custody_id = 6;
  google.protobuf.StringValue custody_name = 7;
  repeated string blockchain_ids = 8;
  int64 broker_account_update_id = 9;
}

```
Response is the stream of historical broker account updates and real time updates as well:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
- | `id` | *number* | ID of the broker account
- | `name` | *string* | *body* | Broker account name
- | `accounts_count` | *number* | Number of accounts that are attached to the broker account
- | `blockchains_count` | *number* | Number of already created broker account details
- | `state` | *[BrokerAccountState](#brokeraccountstate-enum)* | Status of the broker account
- | `created_at` | *timestamp* | Date of the broker account creation
- | `updated_at` | *timestamp* | Date of the latest broker account update
- | `custody_id` | *number* | *body* | ID of the custody for broker account
- | `custody_name` | *string* | *body* | Name of the custody related to broker account
- | `blockchain_ids` | *Array of string* | *body* | Blockchains IDs that are connected to the broker account
- | `aml_connection_ids` | *Array of numbers* | *body* | AML Connections that are enabled for the broker account
- | `broker_account_update_id` | *number* | *body* | Broker account update ID


