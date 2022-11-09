# Custodies

A Custody is a place where private keys of your blockchain wallets are generated and kept, where your transaction signing policy can be edited and where it executes. In order to create any broker-account or account, receive or send transactions in Sirius, you have to create a Custody first. There are two types of the Custody:

* Self-Managed - can be run and managed either by **SwissChain** or by the **Customer** on infrastructure owned and controlled by the Customer. SwissChain provides Custody as a set of Docker images. It's fully secure and protected solution to keep your funds save and under your control. SwissChain, IBM admins, your admins or anyone else can't get unauthorized access to your funds. The solutions is based on **IBM HSM**, **Hyper Protect Virtual Servers**, **Hyper Protected DBaaS**, **Secure Image Build**, **Secure Image Deploy**, and **Operational Decision Manager**. It provides a flexible way to control you transaction signing policy. It includes **Multi Party Signature** mechanism which allows you to delegate transaction signing to any number of your employees and/or software components. The same time transactions can be signed automatically still. You can configure this via our transaction signing policy engine. It can be run either in the cloud or on-premise. IBM HSM is the only solution in the industry which is **FIPS 140-2 Level 4-certified**. [Contact](mailto:contact@swisschain.io) us to get help with deploying of the self-managed Custody.
* Shared - run and managed by SwissChain, shared between all the customers that use it.

We recommend using Shared Custody for the testing purposes only.

## Custody creation

### Request

**Rest API:** `POST /api/custodies`

**gRPC API:** `swisschain.sirius.api.custodies.Custodies/Create`

```json
POST /api/custodies

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "name": "My Custody",
  "type": "private"
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/Create

> Requets: (application/grpc)

message CustodyCreateRequest {
  string request_id = 1;    // 1a5c0b3d15494ec8a390fd3b22d757d6
  string name = 2;          // "My Custody"
  CustodyType type = 3;     // CustodyType.PRIVATE
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`name` | `name` | *string* | *body* | Name of the custody
`type` | `type` | *[CustodyType](#custodytype-enum)* | *body* | Type of the custody

### Response

```json
POST /api/custodies 

> Response: 200 (application/json) - success response

{
  "id": 100010,
  "name": "My Custody",
  "type": "private",
  "status": "offline",
  "createdAt": "2020-08-24T21:43:02.6554641Z",
  "updatedAt": "2020-08-24T21:43:02.6554641Z"
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/Create

> Response: (application/grpc) - success response

message CustodyCreateResponse {
    oneof result {
      CustodyCreateResponseBody body = 1;                       // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2; // NULL
    } 
}

message CustodyCreateResponseBody {
  CustodyResponse custody = 1;                                  // Object
}

message CustodyResponse {
  int64 id = 1;                                                 // 100010
  string name = 2;                                              // "My Custody"
  CustodyType type = 3;                                         // CustodyType.PRIVATE
  CustodyStatus status = 4;                                     // CustodyStatus.OFFLINE
  google.protobuf.Timestamp created_at = 5;                     // "2020-08-24T21:43:02.6554641Z"
  google.protobuf.Timestamp updated_at = 6;                     // "2020-08-24T21:43:02.6554641Z"
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the custody
`name` | `name` | *string* | Name of the custody
`type` | `type` | *[CustodyType](#custodytype-enum)* | Type of the custody
`status` | `status` | *[CustodyStatus](#custodystatus-enum)* | Status of the custody
`createdAt` | `created_at` | *timestamp* | Date of the custody creation
`updatedAt` | `updated_at` | *timestamp* | Date of the custody update

## Custody update

### Request

**Rest API:** `PUT /api/custodies/{custodyId}`

**gRPC API:** `swisschain.sirius.api.custodies.Custodies/Update`

```json
PUT /api/custodies/100010

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "name": "My Secure Custody"
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/Update

> Requets: (application/grpc)

message CustodyUpdateRequest {
  string request_id = 1;    // 1a5c0b3d15494ec8a390fd3b22d757d6
  int64 custody_id = 2;     // 100010
  string name = 3;          // "My Secure Custody"
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`custodyId` | `custody_id` | *number* | *path* | ID of the custody being update
`name` | `name` | *string* | *body* | Name of the custody to set

### Response

```json
PUT /api/custodies/100010

> Response: 200 (application/json) - success response

{
  "id": 100010,
  "name": "My Secure Custody",
  "type": "private",
  "status": "offline",
  "createdAt": "2020-08-24T21:43:02.6554641Z",
  "updatedAt": "2020-08-24T21:43:02.6554641Z"
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/Update

> Response: (application/grpc) - success response

message CustodyUpdateResponse {
  oneof result {
    CustodyUpdateResponseBody body = 1;                         // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;   // NULL
  } 
}

message CustodyUpdateResponseBody {
  CustodyResponse custody = 1;                                  // Object
}

message CustodyResponse {
  int64 id = 1;                                                 // 100010
  string name = 2;                                              // "My Secure Custody"
  CustodyType type = 3;                                         // CustodyType.PRIVATE
  CustodyStatus status = 4;                                     // CustodyStatus.OFFLINE
  google.protobuf.Timestamp created_at = 5;                     // "2020-08-24T21:43:02.6554641Z"
  google.protobuf.Timestamp updated_at = 6;                     // "2020-08-24T21:43:02.6554641Z"
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the custody
`name` | `name` | *string* | Name of the custody
`type` | `type` | *[CustodyType](#custodytype-enum)* | Type of the custody
`status` | `status` | *[CustodyStatus](#custodystatus-enum)* | Status of the custody
`createdAt` | `created_at` | *timestamp* | Date of the custody creation
`updatedAt` | `updated_at` | *timestamp* | Date of the custody update

## Custodies search

### Request

**Rest API:** `GET /api/custodies`

**gRPC API:** `swisschain.sirius.api.custodies.Custodies/Search`

```json
GET /api/custodies?id=100010&name=My%20Custody&type=private&order=asc&cursor=100009&limit=3
```

```protobuf
swisschain.sirius.api.custodies.Custodies/Search

> Requets: (application/grpc)

message CustodySearchRequest {  
  google.protobuf.Int64Value id = 1;                            // 100010
  google.protobuf.StringValue name = 2;                         // "My Custody"
  repeated CustodyType type = 3;                                // Array<CustodyType>
  swisschain.sirius.api.common.PaginationInt64 pagination = 4;  // Object
}

message PaginationInt64 {
  PaginationOrder order = 1;                                    // PaginationOrder.ASC
  google.protobuf.Int64Value cursor = 2;                        // 100009
  int32 limit = 3;                                              // 3
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`id` | `id` | *number, optional* | *query* | Exact ID of the custody to search
`name` | `name` | *string, optional* | *query* | Name of the custody to search
`type` | `type` | *Array<[CustodyType](#custodytype-enum)>, optional* | *query* | Zero or several custody types to search
`order` | `pagination.order` | *[Order](#order-enum), optional* | *query* | Results sorting order
`cursor` | `pagination.cursor` | *number, optional* | *query* | Cursor to continue search from
`limit` | `pagination.limit` | *number, optional* | *query* | Max numbers of the items to return from the server

### Response

```json
GET /api/custodies?&name=Custody&type=private&order=asc&cursor=100009&limit=3

> Response: 200 (application/json) - success response

{
  "pagination": {
    "cursor": 100009,
    "count": 3,
    "order": "asc",
    "nextUrl": "/api/custodies?&name=Custody&type=private&order=asc&cursor=100012&limit=3"
  },
  "items": [
    {
      "id": 100010,
      "name": "My Custody",
      "type": "shared",
      "status": "online",
      "createdAt": "2020-08-27T22:08:56.976533Z",
      "updatedAt": "2020-08-27T22:08:56.976533Z"
    },
    {
      "id": 100011,
      "name": "My Secure Custody",
      "type": "private",
      "status": "online",
      "createdAt": "2020-08-27T22:08:56.976533Z",
      "updatedAt": "2020-08-27T22:08:56.976533Z"
    },
    {
      "id": 100012,
      "name": "Test Custody",
      "type": "shared",
      "status": "online",
      "createdAt": "2020-08-27T22:08:56.976533Z",
      "updatedAt": "2020-08-27T22:08:56.976533Z"
    }
  ]
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/Search

> Response: (application/grpc) - success response

message CustodySearchResponse {
  oneof result {
    CustodySearchResponseBody body = 1;                                 // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;           // NULL
  } 
}

message CustodySearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated CustodyResponse items = 2;                                   // Array<Object>
}

message CustodyUpdateResponseBody {
  CustodyResponse custody = 1;                                          // Object
}

message PaginatedInt64Response {
  google.protobuf.Int64Value cursor = 1;                                // 100009
  int32 count = 2;                                                      // 3
  PaginationOrder order = 3;                                            // PaginationOrder.ASC
}

message CustodyResponse {
  int64 id = 1;                                                         // 100010
  string name = 2;                                                      // "My Secure Custody"
  CustodyType type = 3;                                                 // CustodyType.PRIVATE
  CustodyStatus status = 4;                                             // CustodyStatus.OFFLINE
  google.protobuf.Timestamp created_at = 5;                             // "2020-08-24T21:43:02.6554641Z"
  google.protobuf.Timestamp updated_at = 6;                             // "2020-08-24T21:43:02.6554641Z"
}
```

[Paginated](#Pagination) list of

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the custody
`name` | `name` | *string* | Name of the custody
`type` | `type` | *[CustodyType](#custodytype-enum)* | Type of the custody
`status` | `status` | *[CustodyStatus](#custodystatus-enum)* | Status of the custody
`createdAt` | `created_at` | *timestamp* | Date of the custody creation
`updatedAt` | `updated_at` | *timestamp* | Date of the custody update

## Custody API key creation

### Request

**Rest API:** `POST /api/custodies/{custodyId}/api-keys`

**gRPC API:** `swisschain.sirius.api.custodies.Custodies/AddApiKey`

```json
POST /api/custodies/100010/api-keys

> Request: (application/json)

x-request-id: d5125d2d4d9445efa5414a3926ab00a7

{
  "name": "API key for my custody",
  "expiresAt": "2020-09-24T21:43:02.6554641Z"
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/AddApiKey

> Requets: (application/grpc)

message CustodyAddApiKeyRequest {
  string request_id = 1;                        // d5125d2d4d9445efa5414a3926ab00a7
  int64 custody_id = 2;                         // 100010
  string name = 3;                              // "API key for my custody"
  google.protobuf.Timestamp expires_at = 4;     // "2020-09-24T21:43:02.6554641Z"
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`custodyId` | `custody_id` | *number* | *path* | ID of the custody being update
`name` | `name` | *string* | *body* | Name of the custody to set
`expiresAt` | `expires_at` | *timestamp* | *body* | Date of the API key expiration

### Response

```json
POST /api/custodies/100010/api-keys

> Response: 200 (application/json) - success response

{
  "id": 100014,
  "custodyId": 100010,
  "name": "API key for my custody",
  "expiresAt": "2020-09-24T21:43:02.6554641Z",
  "issuedAt": "2020-08-24T21:43:02.6554641Z",
  "isRevoked": false
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/AddApiKey

> Response: (application/grpc) - success response

message CustodyAddApiKeyResponse {
  oneof result {
    CustodyAddApiKeyResponseBody body = 1;                          // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;       // NULL
  } 
}

message CustodyAddApiKeyResponseBody {
  CustodyApiKeyResponse custody_api_key = 1;                        // Object
}

message CustodyApiKeyResponse {
  int64 id = 1;                                                     // 100014
  int64 custody_id = 2;                                             // 100010
  string name = 3;                                                  // "API key for my custody"
  google.protobuf.Timestamp expires_at = 4;                         // "2020-09-24T21:43:02.6554641Z"
  google.protobuf.Timestamp issued_at = 5;                          // "2020-08-24T21:43:02.6554641Z"
  bool is_revoked = 6;                                              // FALSE
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the custody API key
`custodyId` | `custody_id` | *number* | ID of the custody
`name` | `name` | *string* | Name of the custody
`expiresAt` | `expires_at` | *timestamp* | Date of the API key expiration
`issuedAt` | `issued_at` | *timestamp* | Date of the API key issuance
`isRevoked` | `is_revoked` | *bool* | Flag indicating if the API key is revoked

## Custody API keys search

### Request

**Rest API:** `GET /api/custodies/{custodyId}/api-keys`

**gRPC API:** `swisschain.sirius.api.custodies.Custodies/SearchApiKeys`

```json
GET /api/custodies/100010/api-keys?id=100014&name=API%20key%20for%20my%20custody&isRevoked=false&order=asc&cursor=100013&limit=3
```

```protobuf
swisschain.sirius.api.custodies.Custodies/SearchApiKeys

> Requets: (application/grpc)

message CustodySearchApiKeyRequest {  
  google.protobuf.Int64Value id = 1;                            // 100014
  google.protobuf.StringValue name = 2;                         // "API key for my custody"
  int64 custody_id = 4;                                         // 100010
  google.protobuf.BoolValue is_revoked = 5;                     // false
  swisschain.sirius.api.common.PaginationInt64 pagination = 6;  // Object
}

message PaginationInt64 {
  PaginationOrder order = 1;                                    // PaginationOrder.ASC
  google.protobuf.Int64Value cursor = 2;                        // 100013
  int32 limit = 3;                                              // 3
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`custodyId` | `custody_id` | *number* | *path* | Exact ID of the custody to search
`id` | `id` | *number, optional* | *query* | Exact ID of the custody API key to search
`name` | `name` | *string, optional* | *query* | Name of the custody API key to search
`isRevoked` | `is_revoked` | *bool, optional* | *query* | Flag indicating if only revoked or only not revoked custody API keys should be searched
`order` | `pagination.order` | *[Order](#order-enum), optional* | *query* | Results sorting order
`cursor` | `pagination.cursor` | *number, optional* | *query* | Cursor to continue search from
`limit` | `pagination.limit` | *number, optional* | *query* | Max numbers of the items to return from the server

### Response

```json
GET /api/custodies/100010/api-keys?name=API%20key&isRevoked=false&order=asc&cursor=100013&limit=3

> Response: 200 (application/json) - success response

{
  "pagination": {
    "cursor": 100009,
    "count": 3,
    "order": "asc",
    "nextUrl": /api/custodies/100010/api-keys?name=API%20key&isRevoked=false&order=asc&cursor=100016&limit=3
  },
  "items": [
    {
      "id": 100014,
      "custodyId": 100010,
      "name": "API key for my custody",
      "expiresAt": "2021-09-10T20:26:22.845Z",
      "issuedAt": "2020-09-10T20:26:22.845Z",
      "isRevoked": false
    },
    {
      "id": 100015,
      "custodyId": 100010,
      "name": "Another one API key for my custody",
      "expiresAt": "2021-09-10T20:26:22.845Z",
      "issuedAt": "2020-09-10T20:26:22.845Z",
      "isRevoked": false
    },
    {
      "id": 100016,
      "custodyId": 100010,
      "name": "One more API key for my custody",
      "expiresAt": "2021-09-10T20:26:22.845Z",
      "issuedAt": "2020-09-10T20:26:22.845Z",
      "isRevoked": false
    }
  ]
}
```

```protobuf
swisschain.sirius.api.custodies.Custodies/SearchApiKeys

> Response: (application/grpc) - success response

message CustodySearchApiKeyResponse {
  oneof result {
    CustodySearchApiKeyResponseBody body = 1;                           // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;           // NULL
  } 
}

message CustodySearchApiKeyResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated CustodyApiKeyResponse items = 2;                             // Array<Object>
}

message CustodyApiKeyResponse {
  int64 id = 1;                                                         // 100016
  int64 custody_id = 2;                                                 // 100010
  string name = 3;                                                      // "One more API key for my custody"
  google.protobuf.Timestamp expires_at = 4;                             // "2021-09-10T20:26:22.845Z"
  google.protobuf.Timestamp issued_at = 5;                              // "2020-09-10T20:26:22.845Z"
  bool is_revoked = 6;                                                  // FALSE
}
```

[Paginated](#Pagination) list of

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the custody API key
`custodyId` | `custody_id` | *number* | ID of the custody
`name` | `name` | *string* | Name of the custody API key
`expiresAt` | `expires_at` | *timestamp* | Date of the API key expiration
`issuedAt` | `issued_at` | *timestamp* | Date of the API key issuance
`isRevoked` | `is_revoked` | *bool* | Flag indicating if the custody API key is revoked

## Custody API key token obtaining

### Request

**Rest API:** `GET /api/custodies/{custodyId}/api-keys/{apiKeyId}/token`

#### Parameters

#### Query Parameters

### Response

> GET /api/custodies/{custodyId}/api-keys/{apiKeyId}/token 200 OK

```json
{
}
```

## Custody API key token revocation

### Request

**Rest API:** `POST /api/custodies/{custodyId}/api-keys/{apiKeyId}/revoke`

#### Parameters

#### Query Parameters

### Response

> POST /api/custodies/{custodyId}/api-keys/{apiKeyId}/revoke 200 OK

```json
{
}
```