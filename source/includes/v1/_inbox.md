# Inbox

Withdrawal refers to outgoing transfers of the funds from a broker account. You can get withdrawals history, or be notified about new withdrawals and state changing of the existing withdrawals. There are different options to be notified about withdrawals state changing:

* You can pull withdrawals updates REST API
* You can listen for withdrawal updates gRPC stream
* You can register a web-hook get the notifications

Withdrawal can be initiated by you via API or Universe portal UI. Every withdrawal is associated with a broker account, an asset, optionally an account, your internal ID, it has amount, fees and state. If you use reference ID for the accounts, you can specify either account ID or reference ID, to associate an account to the withdrawal. This link doesn't affect funds flow, it's used just for the reporting and notifying you back with withdrawal updates.

The fees required for the withdrawal transaction are paid atop of the specified amount.

The lifecycle of a withdrawal:

<img src="https://raw.githubusercontent.com/swisschain/Sirius.Api.Docs/master/source/images/withdrawal-lifecycle.png" alt="Withdrawal lifecycle"/>

## Starts a withdrawal

### Request

**Rest API:** `POST /api/withdrawals`
**gRPC API:** `swisschain.sirius.api.withdrawals.Withdrawals.Execute`


```json
POST /api/withdrawals

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "document": "{\"version\":\"1.0\",\"brokerAccountId\":100000162,\"accountId\":103000171,\"accountReferenceId\":99,\"withdrawalReferenceId\":76766,\"assetId\":112248,\"amount\":\"0.053\",\"destinationDetails\":{\"address\":\"0x45238387503b485ae1a279d8B420f0e8562bfa35\",\"tag\":null,\"tag_type\":null}}",
  "signature": "CBAirYR2d6vmRV6d1GEfdA3Kv11pOvdebhEyx8pGNKvTaQcbc0IiBUKCESX9eiOlxorkMDuOEMVs4HZ6JnKg6aY8akb72723cBTxvNIHb/P3OyHQWECcIKwmruWXEunWS8ZlFjxy/nWr/XGD0G8N8/xREGhGCcHCjdwrMmMQBSg="
}


```

```protobuf
swisschain.sirius.api.withdrawals.Withdrawals.Execute

> Requets: (application/grpc)

message WithdrawalExecuteRequest {
    string request_id = 1; // 3a2b59a1-07e5-48b1-ba8b-4b3c750a2aa1
    string document = 2; // "{\"version\":\"1.0\",\"brokerAccountId\":100000162,\"accountId\":103000171,\"accountReferenceId\":99,\"withdrawalReferenceId\":76766,\"assetId\":112248,\"amount\":\"0.053\",\"destinationDetails\":{\"address\":\"0x45238387503b485ae1a279d8B420f0e8562bfa35\",\"tag\":null,\"tag_type\":null}}"
    string signature = 3; // "CBAirYR2d6vmRV6d1GEfdA3Kv11pOvdebhEyx8pGNKvTaQcbc0IiBUKCESX9eiOlxorkMDuOEMVs4HZ6JnKg6aY8akb72723cBTxvNIHb/P3OyHQWECcIKwmruWXEunWS8ZlFjxy/nWr/XGD0G8N8/xREGhGCcHCjdwrMmMQBSg="
}

```

### Parameters

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`document` | `document` | *optional*, *serialized-as-string*, *[WithdrawalDocument](#withdrawaldocument-object)* | *body* | JSON-formatted document describing the withdrawals parameters serialized as string
`signature` | `signature` | *optional*, *string* | *body* | Base64-encoded RSA signature of the document signed with the Customer's private key

```
Withdral document format:

{
    "version": "string", 				// "1.0"
    "brokerAccountId": 1000000,
    "accountId": 1000000, 				// Optional
    "accountReferenceId": "string",		// Optional
    "withdrawalReferenceId": "string",	// Optional
    "assetId": 1000000,
    "amount": "decimal", 				// 12.345,
    "destinationDetails": {
        "address": "string",			// Blockchain address
        "tag": "string", 				// Optional
        "tagType": "string", 			// Optional - ("text", "number")
    }
}
```

### Query Parameters

### Response

```json
POST /api/withdrawals

> Response: 200 (application/json) - success response

{
 "id":106000003,
 "brokerAccountId":100000005,
 "brokerAccountName":"Hot wallet",
 "brokerAccountDetails":  {
  "id":101000015,
  "address":"0xB335c0a4c4D07164788682Abb9806B0aBd8AB096"
   },
 "accountId":null,
 "accountDetails":null,
 "assetId":112248,
 "assetSymbol":"ETH",
 "assetAddress":null,
 "amount":0.0001,
 "blockchainId":"ethereum-ropsten",
 "blockchainName":"Ethereum Ropsten",
 "fees":[],
 "destinationDetails":{
  "address":"0x45238387503b485ae1a279d8B420f0e8562bfa35",
  "tag":null,
  "tagType":null
  },
 "state":"processing",
 "transactionInfo":null,
 "error":null,
 "operationId":null,
 "requiredConfirmationsCount":-1,
 "transferContext": {
  "accountReferenceId":null,
  "withdrawalReferenceId":null,
  "document":"{\"version\":\"1.0\",\"brokerAccountId\":100000005,\"accountId\":null,\"accountReferenceId\":null,\"withdrawalReferenceId\":null,\"assetId\":112248,\"amount\":0.0001,\"destinationDetails\":{\"address\":\"0x45238387503b485ae1a279d8B420f0e8562bfa35\",\"tagType\":null,\"tag\":null}}",
  "signature":null,
  "requestContext":{
   "userId":"67070a1f-033a-4ed9-ad1e-1214291687d0",
   "apiKeyId":null,
   "ip":"192.168.16.36",
   "timestamp":"2021-03-31T11:46:46.059629Z"
   }
  },
  "validatorContext":null,
  "createdAt":"2021-03-31T11:46:46.614955Z",
  "updatedAt":"2021-03-31T11:46:46.614955Z"
}
```

```protobuf
swisschain.sirius.api.withdrawals.Withdrawals.Execute

> Response: (application/grpc) - success response

message WithdrawalExecuteResponse {
    oneof result {
        WithdrawalExecuteResponseBody body = 1;
        WithdrawalExecuteErrorResponseBody error = 2;
    }
}

message WithdrawalExecuteResponseBody {
    WithdrawalResponse withdrawal = 1;
}

message WithdrawalResponse {
    int64 id = 1;
    int64 broker_account_id = 2;
    string broker_account_name = 3;
    swisschain.sirius.api.common.BrokerAccountDetails broker_account_details = 4;
    google.protobuf.Int64Value account_id = 5;
    swisschain.sirius.api.common.AccountDetails account_details = 6;
    google.protobuf.StringValue account_reference_id = 7;
    int64 asset_id = 8;
    string asset_symbol = 9;
    google.protobuf.StringValue asset_address = 10;
    swisschain.sirius.api.common.BigDecimal amount = 11;
    string blockchain_id = 12;
    string blockchain_name = 13;
    repeated swisschain.sirius.api.common.Fee fee = 14;
    DestinationDetails destination_details = 15;
    WithdrawalState state = 16;
    swisschain.sirius.api.common.TransactionInfo transaction_info = 17;
    WithdrawalError error = 18;
    google.protobuf.Int64Value operation_id = 19;
    int64 required_confirmations_count = 20;
    TransferContext transfer_context = 21;
    ValidatorContext validator_context = 22;
    google.protobuf.Timestamp created_at = 23;
    google.protobuf.Timestamp updated_at = 24;
}
```
### Parameters

REST name | gRPC name | type | description 
--------- | --------- | ---- | -------------- | -----------
`id ` | `id ` | *number* | ID of the withdrawal
`brokerAccountId` | `broker_account_id` | *number* |ID of the broker account
`brokerAccountName` | `broker_account_name` | *string* | Name of the broker account
`brokerAccountDetails` | `broker_account_details` | *string* | Details of broker account wallet
`accountId` | `account_id` | *number* |ID of the account
`accountDetails` | `account_details` | *string* | Details of account wallet
 - | `account_reference_id ` | *string* | Reference of account
`assetId` | `asset_id` | *number* | ID of the asset
`assetSymbol` | `asset_symbol` | *string* |Symbol of the asset
`assetAddress` | `asset_address` | *string* | Address of the asset (for ERC20/223)
`amount` | `amount` | *decimal* | Withdrawal amount
`blockchainId` | `blockchain_id` | *number* | Id of blockhain used for withdrawal
`blockchainName` | `blockchain_name` | *string* | Name of blockhain used for withdrawal
`fees` | `fee` | *decimal* | Fee details
`destinationDetails` | `destination_details` | *string* | Details of destination wallet
`state` | `state` | *string* | State of withdrawal
`transactionInfo` | `transaction_info` | *string* |Unique ID of the request
`error` | `error` | *string* | Error details if withdrawal failed
`operationId` | `operation_id` | *number* | Id of operation
`requiredConfirmationsCount` | `required_confirmations_count` | *number* | -
`transferContext` | `transfer_context` | *string* | Details of initial transfer with document
`validatorContext` | `validator_context` | *string* | Details of validator response
`createdAt` | `created_at` | *timestamp* | Datetime of withdrawal creation
`updatedAt` | `updated_at` | *timestamp* | Datetime of latest withdrawal update


## Searches withdrawals

### Request

**Rest API:** `GET /api/withdrawals`
**gRPC API:** `swisschain.sirius.api.withdrawals.Withdrawals.Search`

```protobuf
swisschain.sirius.api.withdrawals.Withdrawals.Search

> Requets: (application/grpc)

{
  "id": {
    "value": 106000107
  },
  "broker_account_id": {
    "value": 100000062
  },
  "account_id": {
    "value": 103001270
  },
  "asset_id": {
    "value": 300000004
  },
  "blockchain_id": {
    "value": "ethereum-ropsten"
  },
  "state": [ ],
  "transaction_id": null,
  "error_code": [ ],
  "operation_id": null,
  "destination_address": {
    "value": "0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71"
  },
  "destination_tag": null,
  "destination_tag_type": null,
  "pagination": {
    "order": 0,
    "cursor": {
      "value": 0
    },
    "limit": 10
  },
  "account_reference_id": {
    "value": "newerc"
  },
  "user_id": {
    "value": 108000015
  },
  "user_native_id": {
    "value": "us1"
  }
}


message WithdrawalSearchRequest {
    google.protobuf.Int64Value id = 1;
    google.protobuf.Int64Value broker_account_id = 2;
    google.protobuf.Int64Value account_id = 3;
    google.protobuf.Int64Value asset_id = 4;
    google.protobuf.StringValue blockchain_id = 5;
    repeated WithdrawalState state = 6;
    google.protobuf.StringValue transaction_id = 7;
    repeated WithdrawalErrorCode error_code = 8;
    google.protobuf.Int64Value operation_id = 9;
    google.protobuf.StringValue destination_address = 10;
    google.protobuf.StringValue destination_tag = 11;
    swisschain.sirius.api.common.NullableTagType destination_tag_type = 12;
    swisschain.sirius.api.common.PaginationInt64 pagination = 13;
    google.protobuf.StringValue account_reference_id = 14;
    google.protobuf.Int64Value user_id = 15;
    google.protobuf.StringValue user_native_id = 16;
}
```

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`id` | `id` | *optional*, *number* | Exact deposit ID to search | 106000107
`brokerAccountId` | `broker_account_id` | *optional*, *number* | ID of the broker account  | 100000062
`accountId` | `account_id` | *optional*, *number* | ID of the account  | 103001270
`accountReferenceId` | `account_reference_id` | *optional*, *string* | Reference ID of the account  | newerc
`userId` | `user_id` | *optional*, *string* | Reference ID of the user with AML relations | 108000015
`userNativeId` | `user_native_id` | *optional*, *string* | Reference ID or Name of the account inside AML system | us1
`assetId` | `asset_id` | *optional*, *number* | ID of the asset  | 300000004
`blockchainId` | `blockchain_id` | *optional*, *string* | ID of the blockchain  | ethereum-ropsten
`state` | `state` | *optional*, *Array of [WithdrawalState](#withdrawalstate-enum)* | State of the withdrawal | sent
`transactionId` | `transaction_id` | *optional*, *number* | ID of the transaction  | 300000123
`errorCode` | `error_code` | *optional*, *Array of [WithdrawalError](#withdrawalerror-enum)* | Reason of the failed withdrawal | invalidDestinationAddress
`operationId` | `operation_id` | *optional*, *number* | ID of the operation  | 400000543
`destinationAddress` | `destination_address` | *optional*, *string* | Address of destination wallet | 0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71
`destinationTag` | `destination_Tag` | *optional*, *string* or *number*  | Address extension of destination wallet | MyStellarMemo / 200004
`destinationTagType` | `destination_tag_type` | *optional*, *string* or *number* | Select type of destination tag  | Text / Number
`order` | `pagination.order` | *optional*, *[Order](#order)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


### Response

> GET /api/withdrawals 200 OK
> swisschain.sirius.api.withdrawals.Withdrawals.Search Success

```json
{
  "pagination":
  {
    "cursor": null,
    "count": 1,
    "order": "desc",
    "nextUrl": null
  },
  "items":
  [
    {
      "id": 106000107,
      "brokerAccountId": 100000062,
      "brokerAccountName": "broker3",
      "brokerAccountDetails":
      {
        "id": 101000172,
        "address": "0x0DB6947809533142C0D8993FD466A5fa890DB68a"
      },
      "accountId": 103001270,
      "accountReferenceId": "newerc",
      "userId": 108000015,
      "userNativeId": "us1",
      "assetId": 300000004,
      "assetSymbol": "ETH",
      "assetAddress": null,
      "amount": 0.1,
      "blockchainId": "ethereum-ropsten",
      "blockchainName": "Ethereum Ropsten",
      "fees":
      [],
      "destinationDetails":
      {
        "address": "0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71",
        "tag": null,
        "tagType": null
      },
      "state": "completed",
      "transactionInfo":
      {
        "transactionId": "0x64e3283c885ad7a75e7c79de4eddce59db1c8c753da291181c093f45d70e5374",
        "transactionBlock": 11360334,
        "confirmationsCount": 43983,
        "requiredConfirmationsCount": 0,
        "date": "0001-01-01T00:00:00Z"
      },
      "error": null,
      "operationId": 200000270,
      "requiredConfirmationsCount": 0,
      "transferContext":
      {
        "accountReferenceId": "newerc",
        "withdrawalReferenceId": null,
        "document": "{\"version\":\"1.0\",\"brokerAccountId\":100000062,\"accountId\":null,\"accountReferenceId\":\"newerc\",\"withdrawalReferenceId\":null,\"assetId\":300000004,\"amount\":0.1,\"destinationDetails\":
    {\"address\":\"0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71\",\"tagType\":null,\"tag\":null}}",
    "signature": null,
    "requestContext":
    {
      "userId": "0dde3a01-f675-439a-9439-0d1198ddaf8b",
      "apiKeyId": null,
      "ip": "10.244.4.0",
      "timestamp": "2021-11-04T10:46:39.7480342Z"
    }
    },
    "validatorContext":
    {
      "document": "{\"validationRequestId\":419000075,\"tenantId\":\"b25e5938-83d5-4d37-b3d1-858f65bfb52b\",\"validatorResolutions\":
    [{\"validatorId\":\"bEJgk7EgkPhKeoN4r+xI7sLzhLDYRf7IQ5XW86KdN7E=\",\"document\":\"{\\\"resolution\\\":\\\"Approved\\\",\\\"resolutionMessage\\\":\\\"\\\",\\\"transfer\\\":{\\\"blockchain\\\":{\\\"id\\\":\\\"ethereum-ropsten\\\",\\\"protocolId\\\":\\\"ethereum\\\",\\\"networkType\\\":\\\"Test\\\"},\\\"context\\\":{\\\"component\\\":\\\"Brokerage\\\",\\\"document\\\":\\\"{\\\\\\\"version\\\\\\\":\\\\\\\"1.0\\\\\\\",\\\\\\\"brokerAccountId\\\\\\\":100000062,\\\\\\\"accountId\\\\\\\":null,\\\\\\\"accountReferenceId\\\\\\\":\\\\\\\"newerc\\\\\\\",\\\\\\\"withdrawalReferenceId\\\\\\\":null,\\\\\\\"assetId\\\\\\\":300000004,\\\\\\\"amount\\\\\\\":0.1,\\\\\\\"destinationDetails\\\\\\\":{\\\\\\\"address\\\\\\\":\\\\\\\"0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71\\\\\\\",\\\\\\\"tagType\\\\\\\":null,\\\\\\\"tag\\\\\\\":null}}\\\",\\\"documentVersion\\\":null,\\\"operationType\\\":\\\"Withdrawal\\\",\\\"signature\\\":null,\\\"withdrawalReferenceId\\\":null,\\\"requestContext\\\":{\\\"apiKeyId\\\":null,\\\"ip\\\":\\\"10.244.4.0\\\",\\\"timestamp\\\":\\\"2021-11-04T10:46:39.748034200Z\\\",\\\"userId\\\":\\\"0dde3a01-f675-439a-9439-0d1198ddaf8b\\\"}},\\\"destination\\\":{\\\"address\\\":\\\"0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71\\\",\\\"tag\\\":null,\\\"tagType\\\":null},\\\"fee\\\":{\\\"amount\\\":0.000063,\\\"asset\\\":{\\\"address\\\":null,\\\"id\\\":300000004,\\\"symbol\\\":\\\"ETH\\\"}},\\\"id\\\":200000270,\\\"source\\\":{\\\"address\\\":\\\"0x0DB6947809533142C0D8993FD466A5fa890DB68a\\\",\\\"brokerAccount\\\":{\\\"id\\\":100000062,\\\"name\\\":\\\"broker3\\\"}},\\\"value\\\":{\\\"amount\\\":0.1,\\\"asset\\\":{\\\"address\\\":null,\\\"id\\\":300000004,\\\"symbol\\\":\\\"ETH\\\"}}}}\",\"signature\":\"fAF7MzJ0EhsLEqUKtUkZqd19nETlBHMmIg6oz1Ur+JYJYEV37Wjw30rqpciVe7YInKifdzxcvYYlGx+nvgyGjlJATJOsdWSu3nUa/YYbx9eUGX2m18h7FHoQI4CCXBwJkwTJ/aKkbdwkaOzEy6/4eel34xpLeji957Q7DF1j/ig=\",\"resolution\":\"Approved\",\"resolutionMessage\":\"\",\"deviceInfo\":\"{\\\"deviceUID\\\":\\\"8CCC5E5B-38B2-4ACB-B42C-A6713920654F\\\",\\\"platform\\\":\\\"iOS\\\"}\",\"ip\":\"ipv4:10.244.6.0:1728\",\"timestamp\":\"2021-11-04T10:46:55.171321Z\"}],\"requestedValidators\":[\"bEJgk7EgkPhKeoN4r+xI7sLzhLDYRf7IQ5XW86KdN7E=\"],\"status\":\"Approved\",\"rejectReasonMessage\":null,\"timestamp\":\"2021-11-04T10:46:55.263737Z\",\"transfer\":{\"id\":200000270,\"blockchain\":{\"id\":\"ethereum-ropsten\",\"protocolId\":\"ethereum\",\"networkType\":\"Test\"},\"source\":{\"address\":\"0x0DB6947809533142C0D8993FD466A5fa890DB68a\",\"brokerAccount\":{\"id\":100000062,\"name\":\"broker3\"},\"account\":null},\"destination\":{\"address\":\"0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71\",\"tag\":null,\"tagType\":null,\"brokerAccount\":null,\"account\":null},\"value\":{\"asset\":{\"id\":300000004,\"symbol\":\"ETH\",\"address\":null},\"amount\":0.1},\"fee\":{\"asset\":{\"id\":300000004,\"symbol\":\"ETH\",\"address\":null},\"amount\":0.0000630},\"context\":{\"document\":\"{\\\"version\\\":\\\"1.0\\\",\\\"brokerAccountId\\\":100000062,\\\"accountId\\\":null,\\\"accountReferenceId\\\":\\\"newerc\\\",\\\"withdrawalReferenceId\\\":null,\\\"assetId\\\":300000004,\\\"amount\\\":0.1,\\\"destinationDetails\\\":{\\\"address\\\":\\\"0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71\\\",\\\"tagType\\\":null,\\\"tag\\\":null}}\",\"documentVersion\":null,\"signature\":null,\"withdrawalReferenceId\":null,\"component\":\"Brokerage\",\"operationType\":\"Withdrawal\",\"requestContext\":{\"userId\":\"0dde3a01-f675-439a-9439-0d1198ddaf8b\",\"apiKeyId\":null,\"ip\":\"10.244.4.0\",\"timestamp\":\"2021-11-04T10:46:39.748034200Z\"}}}}",
      "signature": "VKwnK35dJaq3ZayUlMtpiZo2T7eCxmwTYHgJbfPjWcwnCzdLLVHU6y5WZaBYgQcVCapZRKnZQUA+5ppjOT2YYZEOUz5vsAWLrIB4U6bJ9/aJ937FnG8rXChWCRzTB/qDxRzv2X/hJFE5RobF9k8M2n6uYi3aHpshX9YUh5qo5Rk="
      },
      "amlChecks":
      {
        "overallResolution": "succeeded",
        "startedAt": "2021-11-04T10:46:40.143002Z",
        "completedChecks":
        [
          {
            "amlCheckId": 602000027,
            "amlConnectionId": 600000010,
            "resolution": "normal",
            "finishedAt": "2021-11-04T10:46:40.2917194Z",
            "description": "Allowed by the whitelist item 603000017 (anyw)"
          }
        ],
        "requiredChecks": 1
      },
      "createdAt": "2021-11-04T10:46:40.00195Z",
      "updatedAt": "2021-11-04T10:48:19.100099Z",
      "sequence": 6
      }
    ]
  }
```

```protobuf
message WithdrawalResponse {
    int64 id = 1;
    int64 broker_account_id = 2;
    string broker_account_name = 3;
    swisschain.sirius.api.common.BrokerAccountDetails broker_account_details = 4;
    google.protobuf.Int64Value account_id = 5;
    google.protobuf.StringValue account_reference_id = 7;
    int64 asset_id = 8;
    string asset_symbol = 9;
    google.protobuf.StringValue asset_address = 10;
    swisschain.sirius.api.common.BigDecimal amount = 11;
    string blockchain_id = 12;
    string blockchain_name = 13;
    repeated swisschain.sirius.api.common.Fee fee = 14;
    DestinationDetails destination_details = 15;
    WithdrawalState state = 16;
    swisschain.sirius.api.common.TransactionInfo transaction_info = 17;
    WithdrawalError error = 18;
    google.protobuf.Int64Value operation_id = 19;
    int64 required_confirmations_count = 20;
    TransferContext transfer_context = 21;
    ValidatorContext validator_context = 22;
    google.protobuf.Timestamp created_at = 23;
    google.protobuf.Timestamp updated_at = 24;
    google.protobuf.Int64Value user_id = 25;
    google.protobuf.StringValue user_native_id = 26;
    swisschain.sirius.api.aml.AmlChecks aml_checks = 27;
}

message TransferContext {
    google.protobuf.StringValue account_reference_id = 1;
    google.protobuf.StringValue withdrawal_reference_id = 2;
    google.protobuf.StringValue document = 3;
    google.protobuf.StringValue signature = 4;
    RequestContext request_context = 5;
}

message RequestContext {
    google.protobuf.StringValue user_id = 1;
    google.protobuf.StringValue api_key_id = 2;
    google.protobuf.StringValue ip = 3;
    google.protobuf.Timestamp timestamp = 4;
}

message ValidatorContext {
    google.protobuf.StringValue document = 1;
    google.protobuf.StringValue signature = 2;
}

```
### Parameters

Paginated response of the withdrawals:

REST name | gRPC name | type | description
--------- | --------- | ---- | -------------- | -----------
`id ` | `id ` | *number* | ID of the withdrawal
`brokerAccountId` | `broker_account_id` | *number* |ID of the broker account
`brokerAccountName` | `broker_account_name` | *string* | Name of the broker account
`brokerAccountDetails` | `broker_account_details` | *string* | Details of broker account wallet
`accountId` | `account_id` | *number* |ID of the account
`accountDetails` | `account_details` | *string* | Details of account wallet
`accountReferenceId` | `account_reference_id ` | *string* | Reference of account
`assetId` | `asset_id` | *number* | ID of the asset
`assetSymbol` | `asset_symbol` | *string* |Symbol of the asset
`assetAddress` | `asset_address` | *string* | Address of the asset (for tokens like ERC20/223)
`amount` | `amount` | *decimal* | Withdrawal amount
`blockchainId` | `blockchain_id` | *number* | Id of blockhain used for withdrawal
`blockchainName` | `blockchain_name` | *string* | Name of blockhain used for withdrawal
`fees` | `fee` | *decimal* | Fee details
`destinationDetails` | `destination_details` | *string* | Details of destination wallet
`state` | `state` | *string* | State of withdrawal
`transactionId` | `transaction_id` | *string* |Unique ID of the request
`error` | `error` | *string* | Error details if withdrawal failed
`operationId` | `operation_id` | *number* | Id of operation
`requiredConfirmationsCount` | `required_confirmations_count` | *number* | -
`transferContext` | `transfer_context` | *string* | Details of initial transfer with document, signature and request context
`validatorContext` | `validator_context` | *string* | Details of validator response
`userId` | `user_id` | *optional*, *string* | Reference ID of the user with AML relations | 108000015
`userNativeId` | `user_native_id` | *optional*, *string* | Reference ID or Name of the account inside AML system | us1
`amlChecks` | `aml_checks` | *string* | Details of aml providers response
`createdAt` | `created_at` | *timestamp* | Datetime of withdrawal creation
`updatedAt` | `updated_at` | *timestamp* | Datetime of latest withdrawal update

## Searches withdrawal updates

### Request

**Rest API:** `GET /api/withdrawal-updates`
**gRPC API:** `swisschain.sirius.api.withdrawals.Withdrawals.GetUpdates`

```protobuf
message WithdrawalUpdateSearchRequest {
google.protobuf.Int64Value withdrawal_id = 1;
google.protobuf.Int64Value broker_account_id = 2;
google.protobuf.Int64Value account_id = 3;
google.protobuf.Int64Value asset_id = 4;
google.protobuf.StringValue blockchain_id = 5;
repeated WithdrawalState state = 6;
google.protobuf.StringValue transaction_id = 7;
repeated WithdrawalErrorCode error_code = 8;
google.protobuf.Int64Value operation_id = 9;
google.protobuf.StringValue destination_address = 10;
google.protobuf.StringValue destination_tag = 11;
swisschain.sirius.api.common.NullableTagType destination_tag_type = 12;
google.protobuf.Int64Value withdrawal_update_id = 13;
google.protobuf.Int64Value cursor = 14;
google.protobuf.StringValue account_reference_id = 15;
google.protobuf.Int64Value user_id = 16;
google.protobuf.StringValue user_native_id = 17;
}

example:
{
  "withdrawal_id": null,
  "broker_account_id": {
    "value": 100000062
  },
  "account_id": null,
  "asset_id": {
    "value": 300000004
  },
  "blockchain_id": {
    "value": "ethereum-ropsten"
  },
  "state": [
    
  ],
  "transaction_id": null,
  "error_code": [
    
  ],
  "operation_id": null,
  "destination_address": null,
  "destination_tag": null,
  "destination_tag_type": null,
  "withdrawal_update_id": null,
  "cursor": {
    "value": 0
  },
  "account_reference_id": null,
  "user_id": null,
  "user_native_id": null
}
```

### Query Parameters

 gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`id` | `withdrawal_id` | *optional*, *number* | Exact deposit ID to search | 106000107
`brokerAccountId` | `broker_account_id` | *optional*, *number* | ID of the broker account  | 100000062
`accountId` | `account_id` | *optional*, *number* | ID of the account  | 103001270
`accountReferenceId` | `account_reference_id` | *optional*, *string* | Reference ID of the account  | newerc
`userId` | `user_id` | *optional*, *string* | Reference ID of the user with AML relations | 108000015
`userNativeId` | `user_native_id` | *optional*, *string* | Reference ID or Name of the account inside AML system | us1
`assetId` | `asset_id` | *optional*, *number* | ID of the asset  | 300000004
`blockchainId` | `blockchain_id` | *optional*, *string* | ID of the blockchain  | ethereum-ropsten
`state` | `state` | *optional*, *Array of [WithdrawalState](#withdrawalstate-enum)* | State of the withdrawal | sent
`transactionId` | `transaction_id` | *optional*, *number* | ID of the transaction  | 300000123
`errorCode` | `error_code` | *optional*, *Array of [WithdrawalError](#withdrawalerror-enum)* | Reason of the failed withdrawal | invalidDestinationAddress
`operationId` | `operation_id` | *optional*, *number* | ID of the operation  | 400000543
`destinationAddress` | `destination_address` | *optional*, *string* | Address of destination wallet | 0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71
`destinationTag` | `destination_Tag` | *optional*, *string* or *number*  | Address extension of destination wallet | MyStellarMemo / 200004
`destinationTagType` | `destination_tag_type` | *optional*, *string* or *number* | Select type of destination tag  | Text / Number
`withdrawalUpdateId` | `withdrawal_update_id` | *optional*, *number* | Exact deposit update ID to search | 600000001
`order` | - | *optional*, *[Order](#Order)* | Result items sorting order | asc
`cursor` | `cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | - | *optional*, *number* | Maximum number of items to return in the search results | 10




### Response


```json
{
  "items": [
    {
      "withdrawal_update_id": "501000593",
      "withdrawal": {
        "fee": [],
        "id": "106000107",
        "broker_account_id": "100000062",
        "broker_account_name": "broker3",
        "broker_account_details": {
          "id": "101000172",
          "address": "0x0DB6947809533142C0D8993FD466A5fa890DB68a"
        },
        "account_id": {
          "value": "103001270"
        },
        "account_reference_id": {
          "value": "newerc"
        },
        "asset_id": "300000004",
        "asset_symbol": "ETH",
        "asset_address": null,
        "amount": {
          "value": "0.1"
        },
        "blockchain_id": "ethereum-ropsten",
        "blockchain_name": "Ethereum Ropsten",
        "destination_details": {
          "address": "0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71",
          "tag": null,
          "tag_type": {
            "null": "NULL_VALUE",
            "kind": "null"
          }
        },
        "state": "PROCESSING",
        "transaction_info": null,
        "error": null,
        "operation_id": null,
        "required_confirmations_count": "-1",
        "transfer_context": {
          "account_reference_id": {
            "value": "newerc"
          },
          "withdrawal_reference_id": null,
          "document": {
            "value": "{\"version\":\"1.0\",\"brokerAccountId\":100000062,\"accountId\":null,\"accountReferenceId\":\"newerc\",\"withdrawalReferenceId\":null,\"assetId\":300000004,\"amount\":0.1,\"destinationDetails\":{\"address\":\"0xBB7cfa448FedDe5c5593ca4487d76A580aFfef71\",\"tagType\":null,\"tag\":null}}"
          },
          "signature": null,
          "request_context": {
            "user_id": {
              "value": "0dde3a01-f675-439a-9439-0d1198ddaf8b"
            },
            "api_key_id": null,
            "ip": {
              "value": "192.168.4.0"
            },
            "timestamp": {
              "seconds": "1636022799",
              "nanos": 748034200
            }
          }
        },
        "validator_context": null,
        "created_at": {
          "seconds": "1636022800",
          "nanos": 1950000
        },
        "updated_at": {
          "seconds": "1636022800",
          "nanos": 1950000
        },
        "user_id": {
          "value": "108000015"
        },
        "user_native_id": {
          "value": "us1"
        },
        "aml_checks": null
      }
    }
  ]
}
```

```protobuf
message WithdrawalUpdateArrayResponse {
repeated WithdrawalUpdateResponse items = 1;
}

message WithdrawalUpdateResponse {
int64 withdrawal_update_id = 1;
WithdrawalResponse withdrawal = 2;
}
```

### Parameters

> GET /api/withdrawal-updates 200 OK
> Stream  swisschain.sirius.api.withdrawals.Withdrawals.GetUpdates

Paginated response of the withdrawals update:

REST name | gRPC name | type | description
--------- | --------- | ---- | -------------- | -----------
`withdrawalUpdateId` | `withdrawal_update_id` | *optional*, *number* | Exact deposit update ID to search | 600000001
`id ` | `id ` | *number* | ID of the withdrawal
`brokerAccountId` | `broker_account_id` | *number* |ID of the broker account
`brokerAccountName` | `broker_account_name` | *string* | Name of the broker account
`brokerAccountDetails` | `broker_account_details` | *string* | Details of broker account wallet
`accountId` | `account_id` | *number* |ID of the account
`accountDetails` | `account_details` | *string* | Details of account wallet
`accountReferenceId` | `account_reference_id ` | *string* | Reference of account
`assetId` | `asset_id` | *number* | ID of the asset
`assetSymbol` | `asset_symbol` | *string* |Symbol of the asset
`assetAddress` | `asset_address` | *string* | Address of the asset (for tokens like ERC20/223)
`amount` | `amount` | *decimal* | Withdrawal amount
`blockchainId` | `blockchain_id` | *number* | Id of blockhain used for withdrawal
`blockchainName` | `blockchain_name` | *string* | Name of blockhain used for withdrawal
`fees` | `fee` | *decimal* | Fee details
`destinationDetails` | `destination_details` | *string* | Details of destination wallet
`state` | `state` | *string* | State of withdrawal
`transactionId` | `transaction_id` | *string* |Unique ID of the request
`error` | `error` | *string* | Error details if withdrawal failed
`operationId` | `operation_id` | *number* | Id of operation
`requiredConfirmationsCount` | `required_confirmations_count` | *number* | -
`transferContext` | `transfer_context` | *string* | Details of initial transfer with document, signature and request context
`validatorContext` | `validator_context` | *string* | Details of validator response
`userId` | `user_id` | *optional*, *string* | Reference ID of the user with AML relations | 108000015
`userNativeId` | `user_native_id` | *optional*, *string* | Reference ID or Name of the account inside AML system | us1
`amlChecks` | `aml_checks` | *string* | Details of aml providers response
`createdAt` | `created_at` | *timestamp* | Datetime of withdrawal creation
`updatedAt` | `updated_at` | *timestamp* | Datetime of latest withdrawal update




