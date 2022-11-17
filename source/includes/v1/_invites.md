# Accounts

Account is the multi-currency (multi-blockchain) account which is associated with a particular broker account. Account has the same blockchains enabled as the parent broker account has. Account can be associated with a thing in your system by using the "reference ID". You can pass ID of your end-user, invoice, contract or any other things with which you want to associate deposits received on the account. Reference ID is optional, so you can avoid passing your internal IDs to Sirius and keep mapping between your objects and Sirius accounts on your side.

## Create an account

### Request

**Rest API:** `POST /api/accounts`

**gRPC API:** `swisschain.sirius.api.accounts.Accounts.Create`

```json
POST /api/accounts

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
    "brokerAccountId": 100000113
    "referenceId": "user reference"
    "userId": 108000004
}
```

```protobuf
swisschain.sirius.api.accounts.Accounts.Create

> Requets: (application/grpc)

message AccountCreateRequest {
  string request_id = 1;                        //1a5c0b3d15494ec8a390fd3b22d757d6

  int64 broker_account_id = 2;                  //100000113
  
  google.protobuf.StringValue reference_id = 3; //user reference (could be empty)

  google.protobuf.Int64Value user_id = 4;       //108000004 (could be empty)
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`brokerAccountId` | `broker_account_id` | *optional*, *number* | *body* | Id of the related Broker Account
`referenceId` | `reference_id` | *optional*, *string* | *body* | Account reference id
`userId` | `user_id` | *optional*, *number* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)

### Response

```json
POST /api/accounts 

> Response: 200 (application/json) - success response

{
    "id":103000158,
    "referenceId":"user reference",
    "brokerAccountId":100000113,
    "state":"creating",
    "createdAt":"2021-03-26T14:14:46.089822+00:00",
    "updatedAt":"2021-03-26T14:14:46.089822+00:00",
    "userId":108000004,
    "userNativeId":"chainalysis-user-1"
}
```

```protobuf
swisschain.sirius.api.accounts.Accounts.Create

> Response: (application/grpc) - success response

message AccountCreateResponse {
    oneof result {
      AccountCreateResponseBody body = 1;                         // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2;   // NULL
    } 
}

message AccountCreateResponseBody {
  AccountResponse account = 1;                                      // Object
}

message AccountResponse {
  int64 id = 1;                                                     // 103000158
  string reference_id = 2;                                          // "user reference" 
  int64 broker_account_id = 3;                                      // 100000113
  AccountStateModel state = 4;                                      // AccountStateModel.Creating
  google.protobuf.Timestamp createdAt = 5;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Timestamp updatedAt = 6;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Int64Value user_id = 7;                           // 108000004
  google.protobuf.StringValue user_native_id = 8;                   // "chainalysis-user-1"
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the account
`referenceId` | `reference_id` | *optional*, *string* | *body* | Account reference id
`brokerAccountId` | `broker_account_id` | *number* | *body* | Id of the related Broker Account
`status` | `status` | *[AccountState](#accountstate-enum)* | Status of the account
`createdAt` | `created_at` | *timestamp* | Date of the account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest account update
`userId` | `user_id` | *optional*, *number* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)
`userNativeId` | `user_native_id` | *optional*, *string* | *body* | Native Id of the user in customer's system

## Update an account

### Request

**Rest API:** `PUT /api/accounts`

**gRPC API:** `swisschain.sirius.api.accounts.Accounts.Update`

```json
POST /api/accounts

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
    "accountId": 100000113
    "userId": 108000004
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
`accountId` | - | *optional*, *number* | *body* | Id of the related Account
`userId` | - | *optional*, *number* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)

### Response

```json
POST /api/accounts 

> Response: 200 (application/json) - success response

{
    "id":103000158,
    "referenceId":"user reference",
    "brokerAccountId":100000113,
    "state":"creating",
    "createdAt":"2021-03-26T14:14:46.089822+00:00",
    "updatedAt":"2021-03-26T14:14:46.089822+00:00",
    "userId":108000004,
    "userNativeId":"chainalysis-user-1"
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the account
`referenceId` | `reference_id` | *optional*, *string* | *body* | Account reference id
`brokerAccountId` | `broker_account_id` | *number* | *body* | Id of the related Broker Account
`status` | `status` | *[AccountState](#accountstate-enum)* | Status of the account
`createdAt` | `created_at` | *timestamp* | Date of the account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest account update
`userId` | `user_id` | *optional*, *number* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)
`userNativeId` | `user_native_id` | *optional*, *string* | *body* | Native Id of the user in customer's system


## Search for accounts

### Request

**Rest API:** `GET /api/accounts`

**gRPC API:** `swisschain.sirius.api.accounts.Accounts.Search`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`id` | `id` | *optional*, *number* | ID of the account to search | 11111111
`brokerAccountId` | `broker_account_id` | *optional*, *number* | Exact broker account id to search | 100000113
`userId` | `user_id` | *optional*, *number* | ID of users | 108000004
`referenceId` | `reference_id` | *optional*, *string* | Account reference id | user reference
`state` | `state` | *optional*, *[AccountState](#accountstate-enum)* | State of the account | creating
`order` | `pagination.order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


```protobuf
swisschain.sirius.api.accounts.Accounts.Search

> Requets: (application/grpc)

message AccountSearchRequest {
  google.protobuf.Int64Value id = 1;                            // 11111111
  google.protobuf.Int64Value broker_account_id = 2;             // 100000113
  repeated AccountStateModel state = 3;                         // [AccountStateModel.Creating, AccountStateModel.Active, AccountStateModel.Blocked]
  google.protobuf.StringValue reference_id = 4;                 // user reference
  swisschain.sirius.api.common.PaginationInt64 pagination = 5;  // Pagination object
  google.protobuf.Int64Value user_id = 6;                       //108000004 (could be empty)
}
```

### Response

```json
GET /api/accounts?id=103000022

> Response: 200 (application/json) - success response

{
    "pagination":
    {
        "cursor":103000021,
        "count":10,
        "order":"asc",
        "nextUrl":"/api/accounts/details?Order=Asc&Cursor=103000031&Limit=10"
    },
    "items":[
        {
            "id":103000022,
            "referenceId":"some ref7",
            "brokerAccountId":100000019,
            "state":"active",
            "createdAt":"2020-09-14T16:25:54.831938+00:00",
            "updatedAt":"2020-09-14T16:28:14.536684+00:00",
            "userId":null,
            "userNativeId":null}, ...]
}
```

```protobuf
swisschain.sirius.api.accounts.Accounts.Search

> Response: (application/grpc) - success response

message AccountSearchResponse {
    oneof result {
      AccountSearchResponseBody body = 1;                         // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2;   // NULL
    } 
}

message AccountSearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Pagination

  repeated AccountResponse items = 2;                                   // Array of objects
}

message AccountResponse {
  int64 id = 1;                                                     // 103000158
  string reference_id = 2;                                          // "user reference" 
  int64 broker_account_id = 3;                                      // 100000113
  AccountStateModel state = 4;                                      // AccountStateModel.Creating
  google.protobuf.Timestamp createdAt = 5;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Timestamp updatedAt = 6;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Int64Value user_id = 7;                           // 108000004
  google.protobuf.StringValue user_native_id = 8;                   // "chainalysis-user-1"
}
```
Paginated response of the accounts:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the account
`referenceId` | `reference_id` | *optional*, *string* | Account reference id
`brokerAccountId` | `broker_account_id` | *number* | Id of the related Broker Account
`status` | `status` | *[AccountState](#accountstate-enum)* | Status of the account
`createdAt` | `created_at` | *timestamp* | Date of the account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest account update
`userId` | `user_id` | *optional*, *number* | Id of the related user in the system (Needed for enabling AML on broker Account)
`userNativeId` | `user_native_id` | *optional*, *string* | Native Id of the user in customer's system

## Searches the account details

### Request

**Rest API:** `GET /api/accounts/{accountId}/details`

**gRPC API:** `swisschain.sirius.api.accounts.Accounts.SearchDetails`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`id` | `id` | *optional*, *number* | ID of the account details to search | 11111111
`accountId` | `account_id` | *optional*, *number* | Exact account id to search | 103000000
`referenceId` | `reference_id` | *optional*, *string* | Account reference id | account reference
`brokerAccountId` | `broker_account_id` | *optional*, *number* | Exact broker account id to search | 103000113
`assetId` | `asset_id` | *optional*, *number* | ID of an asset | 108000004
`blockchainId` | `blockchain_id` | *optional*, *number* | ID of the blockchain | bitcoin-private
`address` | `address` | *optional*, *string* | Address of the account details | bcrt1qnqx8ltjez3jak8f859y20darrhawa8rq73pank
`tag` | `tag` | *optional*, *string* | related tag | null
`tag_type` | `tag_type` | *optional*, *[TagType](#tagtype-enum)* | tag type | null
`order` | `pagination.order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


```protobuf
swisschain.sirius.api.accounts.Accounts.SearchDetails

> Requets: (application/grpc)

message AccountDetailsSearchRequest {
  google.protobuf.Int64Value id = 1;                                    // 11111111
  google.protobuf.Int64Value broker_account_id = 2;                     // 103000113
  google.protobuf.Int64Value account_id = 3;                            // 103000000
  google.protobuf.StringValue reference_id = 4;                         // account reference
  google.protobuf.Int64Value asset_id = 5;                              // 108000004
  google.protobuf.StringValue blockchain_id = 6;                        // bitcoin-private
  google.protobuf.StringValue address = 7;                              // bcrt1qnqx8ltjez3jak8f859y20darrhawa8rq73pank
  google.protobuf.StringValue tag = 8;                                  // null
  repeated .swisschain.sirius.api.common.NullableTagType tag_type = 9;  // NullableTagType.Value.Text
  swisschain.sirius.api.common.PaginationInt64 pagination = 10;         // Pagination object
}
```

### Response

> GET /api/accounts/{accountId}/details 200 OK

```json
{
    "pagination":
    {
        "cursor":null,
        "count":5,
        "order":"asc",
        "nextUrl":null
    },
    "items":[
        {
            "id":104000000,
            "accountId":103000000,
            "createdAt":"2020-09-08T13:10:34.719982+00:00",
            "address":"bcrt1qnqx8ltjez3jak8f859y20darrhawa8rq73pank",
            "blockchainId":"bitcoin-private",
            "tag":null,
            "tagType":null},]}
```

```protobuf
swisschain.sirius.api.accounts.Accounts.SearchDetails

> Response: (application/grpc) - success response

message AccountDetailsSearchResponse {
    oneof result {
      ccountDetailsSearchResponseBody body = 1;
      swisschain.sirius.api.common.ErrorResponseBody error = 2;
  } 
}

message AccountDetailsSearchResponseBody {
   swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;  // Object
   repeated AccountDetailsResponse items = 2;                            // Paginated array of account details
}

message AccountDetailsResponse {
  int64 id = 1;                                                 // 104000000
  int64 account_id = 2;                                         // 103000000
  google.protobuf.Timestamp createdAt = 3;                      // "2020-09-08T13:10:34.719982+00:00"
  string address = 4;                                           // "bcrt1qnqx8ltjez3jak8f859y20darrhawa8rq73pank"
  string blockchain_id = 5;                                     // "bitcoin-private"
  google.protobuf.StringValue tag = 6;                          // null
  swisschain.sirius.api.common.NullableTagType tag_type = 7;    // null
}
```

Paginated array of the account details:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the account details
`blockchainId` | `blockchain_id` | *string* | Blockchain ID
`accountId` | `account_id` | *number* | Id of the related Account
`createdAt` | `created_at` | *timestamp* | Date of the account details creation
`address` | `address` | *number* | Details public address
`tag` | `tag` | *optional*, *string* | Tag which is related to address (applicable for Stellar)
`tagType` | `tag_type` | *optional*, *[TagType](#tagtype-enum)* | Tag type 

## Gets the account details by the asset id

### Request

**Rest API:** `GET /api/accounts/{accountId}/details/by-asset-id/{assetId}`

**gRPC API:** `swisschain.sirius.api.accounts.Accounts.GetDetails`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`accountId` | `account_id` | *number* | ID of the account | 103000000
`assetId` | `asset_id` | *number* | Id of the asset | 100000113


```protobuf
swisschain.sirius.api.accounts.Accounts.GetDetails

> Requets: (application/grpc)

message AccountGetDetailsRequest {
  int64 account_id = 1;                         // 103000000
  int64 asset_id = 2;                           // 100000113
}
```

### Response

```json
GET /api/accounts/103000000/details/by-asset-id/100000113 200 OK

> Response: 200 (application/json) - success response

{
    "id":104000000,
    "accountId":103000000,
    "createdAt":"2020-09-08T13:10:34.719982+00:00",
    "address":"bcrt1qnqx8ltjez3jak8f859y20darrhawa8rq73pank",
    "blockchainId":"bitcoin-private",
    "tag":null,
    "tagType":null
}
```

```protobuf
swisschain.sirius.api.accounts.Accounts.Search

> Response: (application/grpc) - success response

message AccountGetDetailsResponse {
    oneof result {
      AccountGetDetailsResponseBody body = 1;                     // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2;   // Object
    } 
}

message AccountGetDetailsResponseBody {
   AccountDetailsResponse account_detail = 1;                   // Object
}

message AccountDetailsResponse {
  int64 id = 1;                                                 // 104000000
  int64 account_id = 2;                                         // 103000000
  google.protobuf.Timestamp createdAt = 3;                      // "2020-09-08T13:10:34.719982+00:00"
  string address = 4;                                           // "bcrt1qnqx8ltjez3jak8f859y20darrhawa8rq73pank"
  string blockchain_id = 5;                                     // "bitcoin-private"
  google.protobuf.StringValue tag = 6;                          // null
  swisschain.sirius.api.common.NullableTagType tag_type = 7;    // null
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the account details
`blockchainId` | `blockchain_id` | *string* | Blockchain ID
`accountId` | `account_id` | *number* | Id of the related Account
`createdAt` | `created_at` | *timestamp* | Date of the account details creation
`address` | `address` | *number* | Details public address
`tag` | `tag` | *optional*, *string* | Tag which is related to address (applicable for Stellar)
`tagType` | `tag_type` | *optional*, *[TagType](#tagtype-enum)* | Tag type 


## Search for account updates

### Request

**gRPC API:** `swisschain.sirius.api.accounts.Accounts.GetUpdates`

### Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
- | `account_id` | *optional*, *number* | Exact account id to search updates for | 100000113
- | `broker_account_id` | *optional*, *number* | Exact broker account id to search | 103000113
- | `reference_id` | *optional*, *string* | Account reference id | user reference
- | `state` | *optional*, *[AccountState](#accountstate-enum)* | State of the account | creating
- | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110


```protobuf
swisschain.sirius.api.accounts.Accounts.GetUpdates

> Requets: (application/grpc)

message AccountUpdateSearchRequest {
  google.protobuf.Int64Value account_id = 1;            // 100000113
  google.protobuf.Int64Value broker_account_id = 2;     // 103000113
  repeated AccountStateModel state = 3;                 // AccountStateModel.Creating
  google.protobuf.StringValue reference_id = 4;         // user reference
  google.protobuf.Int64Value cursor = 5;                // 108000004 (could be empty)
}
```

### Response

```protobuf
swisschain.sirius.api.accounts.Accounts.GetUpdates

> Response: (application/grpc) - success response

message AccountUpdateArrayResponse{
  repeated AccountUpdateResponse items = 1;
}

message AccountUpdateResponse {
  int64 account_update_id = 1;                          // 104000001        
  int64 account_id = 2;                                 // 103000158
  string reference_id = 3;                              // "user reference" 
  int64 broker_account_id = 4;                          // 100000113
  AccountStateModel state = 5;                          // AccountStateModel.Creating
  google.protobuf.Timestamp createdAt = 6;              // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Timestamp updatedAt = 7;              // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Int64Value user_id = 8;               // 108000004
  google.protobuf.StringValue user_native_id = 9;       // "chainalysis-user-1"
}

```
Response is the stream of historical account updates and real time updates as well:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
- | `account_update_id` | *number* | ID of the account update
- | `id` | *number* | ID of the account
- | `reference_id` | *optional*, *string* | Account reference id
- | `broker_account_id` | *number* | Id of the related Broker Account
- | `status` | *[AccountState](#accountstate-enum)* | Status of the account
- | `created_at` | *timestamp* | Date of the account creation
- | `updated_at` | *timestamp* | Date of the latest account update
- | `user_id` | *optional*, *number* | Id of the related user in the system (Needed for enabling AML on broker Account)
- | `user_native_id` | *optional*, *string* | Native Id of the user in customer's system
