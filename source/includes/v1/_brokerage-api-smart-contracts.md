# SmartContracts
<Placeholder_for_description>

## Deploy a smart contract

### Request

**Rest API:** `POST /api/smart-contracts/deploy`
**gRPC API:** `swisschain.sirius.api.smart_contracts.SmartContracts.Deploy`


```json
POST /api/smart-contracts/deploy

> Request: (application/json, text/plain, */*)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6
x-tfa-code: 000000  


code: (binary)
document: {"name":"mew","referenceId":"qwe","brokerAccountId":100000074,"blockchainId":"ethereum-goerli","arguments":[],"payment":{"assetId":300000010}}
signature: {"JiOtcENJqblnWFcTZ4DiXwUZudlKBUtYnSkUjmfAH/8a1jeMPwFBu8MGjffITh3PRl/VLRTj8kjKWFT8EUR/8Qb3E8dTeuBlCIDU2RlHRRfb6iuTvjlPmUYQ/+Es6oZlH+Tapvjfa57N19poNZ7zimqadsASe3FGt0ME9J5vufE="}


```

```protobuf
swisschain.sirius.api.smart_contracts.SmartContracts.Deploy

> Requets: (application/grpc)

message SmartContractDeployRequest {
  string request_id = 1; // 1a5c0b3d15494ec8a390fd3b22d757d6
  string document = 2; // {"name":"grpc","brokerAccountId":100000071,"blockchainId":"ethereum-ropsten","arguments":[],"payment":{"assetId":300000004}}
  string signature = 3; // mRMfKO7x3TZYkW4uLOrDfhBOAqAY5hIdAyLLHT/BxzrO8Ophwx/qVHlyQevgj4QVS4RnVMQHJaRDAbyIueduScRSoTXCd6XhZ6m8vA3enWogUuW41CX3+2C1GOF9KvJSBznRb6w5AaUbTaiBcixILz7tcE7hqEW2sZt2LSDGcKs=
  bytes code = 4; // binary(BaseSecurityToken.json)
}
```

### Parameters

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`x-request-id` | - | *string* | *header* | Unique ID of the request
`x-2fa-code` | - | *string* | *header* | 2fa code for corresponding Universe user
 - | `request_id` | *string* | - | Unique ID of the request
`document` | `document` | *optional*, *serialized-as-string*, *[SmartContractDocument](#smartcontractdeploymentdocument-object)* | *body* | JSON-formatted document describing the deployment parameters serialized as string
`signature` | `signature` | *optional*, *string* | *body* | Base64-encoded RSA signature of the document signed with the Customer's private key

```

```

### Query Parameters

### Response

```json
POST /api/smart-contracts/deploy

> Response: 200 (application/json) - success response

{
"id": 109000044,
"name": "mew",
"blockchainId": "ethereum-goerli",
"blockchainName": "Ethereum Goerli",
"brokerAccountId": 100000074,
"brokerAccountName": "SCV2New",
"creatorAddress": "0x94afa005a395e470EB56a26E9081dC650EF84732",
"payment":
{
"assetId": 300000010,
"amount": 0.0
},
"arguments":
[],
"state": "processing",
"deploymentContext":
{
"document": "{\"name\":\"mew\",\"referenceId\":\"qwe\",\"brokerAccountId\":100000074,\"blockchainId\":\"ethereum-goerli\",\"arguments\":
[],\"payment\":{\"assetId\":300000010}}",
"codeHash": "0x88604ca3b6d365a9bd9527be0065b6db",
"referenceId": "qwe",
"requestContext":
{
"userId": "0dde3a01-f675-439a-9439-0d1198ddaf8b",
"ip": "192.168.4.0",
"timestamp": "2021-12-09T18:02:42.8053937Z"
}
},
"createdAt": "2021-12-09T18:02:43.0570272Z",
"updatedAt": "2021-12-09T18:02:43.0570272Z"
}
```

```protobuf
`swisschain.sirius.api.smart_contracts.SmartContracts.Deploy`

message SmartContractDeployResponse {
  oneof result {
    SmartContractDeployResponseBody body = 1;
    swisschain.sirius.api.common.ErrorResponseBody error = 2;
  }
}

message SmartContractDeployResponseBody {
  SmartContractModel item = 1;
}


message SmartContractModel {
  enum SmartContractState
  {
    PROCESSING = 0;
    VALIDATING = 1;
    SIGNING = 2;
    SENT = 3;
    ON_CHAIN = 4;
    COMPLETED = 5;
    FAILED = 6;
    REJECTED = 7;
  }
  
  int64 id = 1;
  string name = 2;
  google.protobuf.StringValue address = 3;
  string blockchain_id = 4;
  string blockchain_name = 5;
  google.protobuf.Int64Value broker_account_id = 6;
  google.protobuf.StringValue broker_account_name = 7;
  string creator_address = 8;
  PaymentModel payment = 9;
  repeated smart_connect.data_model.Argument arguments = 10;
  SmartContractState state = 11;
  common.TransactionInfo transaction_info = 12;
  SmartContractDeploymentContext deployment_context = 13;
  ValidatorContext validator_context = 14;
  int64 sequence = 15;
  SmartContractError error = 16;
  google.protobuf.Timestamp created_at = 17;
  google.protobuf.Timestamp updated_at = 18;  
}
```
### Parameters

REST name | gRPC name | type | description 
--------- | --------- | ---- | -------------- | -----------
`id ` | `id ` | *number* | ID of the deployment
`name ` | `name ` | *string* | Name of the smart contract
`address ` | `address ` | *string* | address of the smart contract
`blockchainId` | `blockchain_id` | *number* | Id of blockchain used for deployment
`blockchainName` | `blockchain_name` | *string* | Name of blockchain used for deployment
`brokerAccountId` | `broker_account_id` | *number* |ID of the broker account
`brokerAccountName` | `broker_account_name` | *string* | Name of the broker account
`creator_address ` | `creator_address ` | *string* | address of the contract creator
`payment ` | `payment ` | *PaymentModel* | details for payments executed during deployment
`arguments ` | `arguments ` | *repeated smart_connect.data_model.Argument* | arguments used for deployment
`state` | `state` | *string* | State of deployment
`transactionInfo` | `transaction_info` | *string* | tx details like hash and confirmations count
`deploymentContext` | `deployment_context` | *string* | Details of initial deploy with document
`validatorContext` | `validator_context` | *string* | Details of validator response
`sequence` | `sequence` | *int* | update sequence 
`error` | `error` | *string* | Error details if smart contract failed
`createdAt` | `created_at` | *timestamp* | Datetime of deploy creation
`updatedAt` | `updated_at` | *timestamp* | Datetime of last deploy update


## Parse smart contract metamodel

### Request

**Rest API:** `Post /api/smart-contracts/parse`
**gRPC API:** `swisschain.sirius.api.smart_contracts.SmartContracts.Parse`

```json
POST /api/smart-contracts/parse

> Request: (application/json, text/plain, */*)


code: (binary)
blockchainId: ethereum

```

```protobuf
swisschain.sirius.api.smart_contracts.SmartContracts.Parse


message SmartContractParseRequest{
  string blockchain_id = 1; // ethereum
  bytes code = 2; // BaseSecurityToken.json
}
```



###  Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`blockchainId` | `blockchain_id` | *required*, *string* | ID of the blockchain  | ethereum
`code` | `code` | *required*, *binary* | compiled smart contract

### Response

> POST /api/smart-contracts/parse 200 OK

```json
{
  "constructor":
  {
    "isPayable": false,
    "address": "1e6c5609",
    "parameters":
    []
  },
  "readFunctions":
  [
    {
      "name": "decimals",
      "return":
      {
        "returnType":
        {
          "prototype": "void",
          "constraints":
          []
        }
      },
      "address": "313ce567",
      "parameters":
      [
        {
          "name": "out_1",
          "parameterType":
          {
            "prototype": "int",
            "nativeType": "uint8",
            "constraints":
            [
              {
                "type": "required",
                "errorMessage": "Value is required"
              },
              {
                "type": "maxInt",
                "maxInt": 255,
                "errorMessage": "Max value is 255"
              },
              {
                "type": "minInt",
                "minInt": 0,
                "errorMessage": "Min value is 0"
              }
            ]
          },
          "direction": "out"
        }
      ]
    }, ... 1942 more rows
```

```protobuf
swisschain.sirius.api.smart_contracts.SmartContracts.Parse

message SmartContractParseResponse {
  oneof result {
    SmartContractParseResponseBody body = 1;
    swisschain.sirius.api.common.ErrorResponseBody error = 2;
  }
}

message SmartContractParseResponseBody {
  smart_connect.metamodel.SmartContractInfo metamodel = 1;
}

message SmartContractInfo {
    ConstructorInfo ctor = 1;
    repeated ReadFunctionInfo read_functions = 2;
    repeated WriteFunctionInfo write_functions = 3;
    repeated EventInfo events = 4;
    bytes code = 5; 
}

// Functions

message ConstructorInfo {
    string address = 1;
    bool is_payable = 2;
    repeated ParameterInfo parameters = 3;
}

message ReadFunctionInfo {
    string name = 1;
    string address = 2;
    repeated ParameterInfo parameters = 3;
    ReturnInfo return = 4;
}

message WriteFunctionInfo {
    string name = 1;
    string address = 2;
    bool is_payable = 3;
    repeated ParameterInfo parameters = 4;
    ReturnInfo return = 5;
}

message ParameterInfo {
    string name = 1;
    TypeInfo type = 2;
    ParameterDirection direction = 3;
}

enum ParameterDirection {
    IN = 0;
    OUT = 1;
}

message ReturnInfo {
    string name = 1;
    TypeInfo type = 2;
}

// Events

message EventInfo {
    string name = 1;
    string address = 2;
    TypeInfo type = 3;
}

// Types

message TypeInfo {
  string native_type = 1;
  PrototypeInfo prototype = 2;
  repeated Constraint constraints = 3;
}

message PrototypeInfo {
  oneof kind {
    EnumTypeInfo enum = 1;
    AddressTypeInfo address = 2;
    BoolTypeInfo bool = 3;
    BytesTypeInfo bytes = 4;
    DecimalTypeInfo decimal = 5;
    IntTypeInfo int = 6;
    StringTypeInfo string = 7;
    TimestampTypeInfo timestamp = 8;
    VoidTypeInfo void = 9;
    ArrayTypeInfo array = 10;
    CompositeTypeInfo composite = 11;
    EitherTypeInfo either = 12;
    MapTypeInfo map = 13;
  }
}

message EnumTypeInfo {
    repeated EnumFieldInfo fields = 1;
}

message EnumFieldInfo {
    string name = 1;
    int32 value = 2;
}

message AddressTypeInfo {
}

message BoolTypeInfo {
}

message BytesTypeInfo {
}

message DecimalTypeInfo {
}

message IntTypeInfo {
}

message StringTypeInfo {
}

message TimestampTypeInfo {
}

message VoidTypeInfo {
}

message ArrayTypeInfo {
  TypeInfo element_type = 1;
  repeated ArrayBoundaries dimension_boundaries = 2;
}

message ArrayBoundaries {
  int32 min = 1;
  int32 max = 2;
}

message CompositeTypeInfo {
  repeated FieldInfo components = 1;
}

message FieldInfo {
  string name = 1;
  TypeInfo type = 2;
}

message EitherTypeInfo {
    repeated FieldInfo options = 1;
}

message MapTypeInfo {
    TypeInfo key_type = 1;
    TypeInfo value_type = 2;
}

// Constraints

message Constraint {
    ConstraintType type = 1;
    string error_message = 2;
}
message ConstraintType {
    oneof kind {
        LengthConstraint length = 1;
        MaxDateConstraint max_date = 2;
        MinDateConstraint min_date = 3;
        MaxDecimalConstraint max_decimal = 4;
        MinDecimalConstraint min_decimal = 5;
        MaxIntConstraint max_int = 6;
        MinIntConstraint min_int = 7;
        MaxLengthConstraint max_length = 8;
        MinLengthConstraint min_length = 9;
        RegexConstraint regex = 10;
        RequiredConstraint required = 11;
        UniqueItemsConstraint unique_items = 12;
    }
}

message LengthConstraint {
  int32 length = 1;
}

message MaxDateConstraint {
  google.protobuf.Timestamp max = 1;
}

message MinDateConstraint {
  google.protobuf.Timestamp min = 1;
}

message MaxDecimalConstraint {
  common.BigDecimal max = 1;
}

message MinDecimalConstraint {
  common.BigDecimal min = 1;
}

message MaxIntConstraint {
  common.BigInt max = 1;
}

message MinIntConstraint {
  common.BigInt min = 1;
}

message MaxLengthConstraint {
  int32 max = 1;
}

message MinLengthConstraint {
  int32 min = 1;
}

message RegexConstraint {
  string regex = 1;
}

message RequiredConstraint {  
}

message UniqueItemsConstraint {
} 

```
### Parameters

response of the parsing:
Structured smart contract metadata


## Invoke smart contract method

### Request

**Rest API:** `POST /api/smart-contracts//{smartContractId}/invoke`
**gRPC API:** `swisschain.sirius.api.smart_contracts.SmartContracts.Invoke`


```json
POST /api/smart-contracts/{smartContractId}/invoke

> Request: (application/json, text/plain, */*)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6
x-tfa-code: 000000


document: "{\"methodName\":\"mint\",\"methodAddress\":\"40c10f19\",\"brokerAccountId\":100000074,\"arguments\":[{\"parameterName\":\"account\",\"value\":{\"prototype\":\"address\",\"address\":\"0x94afa005a395e470EB56a26E9081dC650EF84732\"}},{\"parameterName\":\"amount\",\"value\":{\"prototype\":\"int\",\"int\":100000000000000000}}],\"payment\":{\"assetId\":300000010,\"amount\":0}}"
signature: "JiOtcENJqblnWFcTZ4DiXwUZudlKBUtYnSkUjmfAH/8a1jeMPwFBu8MGjffITh3PRl/VLRTj8kjKWFT8EUR/8Qb3E8dTeuBlCIDU2RlHRRfb6iuTvjlPmUYQ/+Es6oZlH+Tapvjfa57N19poNZ7zimqadsASe3FGt0ME9J5vufE="


```


```protobuf

message SmartContractInvokeRequest {
  string request_id = 1;
  string document = 2;
  string signature = 3;
  int64 smart_contract_id = 4;
}


example:
{
  "request_id": "60f0cea8-1836-4ff5-b81d-422c1f705bce",
  "document": "{\"methodName\":\"mint\",\"methodAddress\":\"40c10f19\",\"brokerAccountId\":100000074,\"arguments\":[{\"parameterName\":\"account\",\"value\":{\"prototype\":\"address\",\"address\":\"0x94afa005a395e470EB56a26E9081dC650EF84732\"}},{\"parameterName\":\"amount\",\"value\":{\"prototype\":\"int\",\"int\":100000000000000000}}],\"payment\":{\"assetId\":300000010,\"amount\":0}}",
  "signature": "JiOtcENJqblnWFcTZ4DiXwUZudlKBUtYnSkUjmfAH/8a1jeMPwFBu8MGjffITh3PRl/VLRTj8kjKWFT8EUR/8Qb3E8dTeuBlCIDU2RlHRRfb6iuTvjlPmUYQ/+Es6oZlH+Tapvjfa57N19poNZ7zimqadsASe3FGt0ME9J5vufE=",
  "smart_contract_id": 109000043
}
```

### Query Parameters

 gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
 `smartContractId` | - | *string* | *path* |  ID of the deployed smart contract
 `x-request-id` | - | *string* | *header* | Unique ID of the request
 `x-2fa-code` | - | *string* | *header* | 2fa code for corresponding Universe user
- | `request_id` | *string* | - | Unique ID of the request
  `document` | `document` | *optional*, *serialized-as-string*, *[SmartContractInvocationDocument](#smartcontractinvocationdocument-object)* | *body* | JSON-formatted document describing the invocation parameters serialized as string
  `signature` | `signature` | *optional*, *string* | *body* | Base64-encoded RSA signature of the document signed with the Customer's private key
  - | `smart_contract_id` | *string* | - |  ID of the deployed smart contract



### Response


```json
{
  "id": 110000034,
  "smartContractId": 109000043,
  "smartContractName": "goerlisec",
  "smartContractAddress": "0xa4c8d6d0f64965d3083137e2ab5eaa709f389089",
  "blockchainId": "ethereum-goerli",
  "blockchainName": "Ethereum Goerli",
  "methodName": "mint",
  "methodAddress": "40c10f19",
  "brokerAccountId": 100000074,
  "brokerAccountName": "SCV2New",
  "brokerAccountAddress": "0x94afa005a395e470EB56a26E9081dC650EF84732",
  "payment":
  {
    "assetId": 300000010,
    "amount": 0.0
  },
  "arguments":
  [
    {
      "parameterName": "account",
      "value":
      {
        "prototype": "address",
        "address": "0x94afa005a395e470EB56a26E9081dC650EF84732"
      }
    },
    {
      "parameterName": "amount",
      "value":
      {
        "prototype": "int",
        "int": 100000000000000
      }
    }
  ],
  "state": "processing",
  "invocationContext":
  {
    "document": "{\"methodName\":\"mint\",\"methodAddress\":\"40c10f19\",\"brokerAccountId\":100000074,\"referenceId\":\"example\",\"arguments\":
[{\"parameterName\":\"account\",\"value\":{\"prototype\":\"address\",\"address\":\"0x94afa005a395e470EB56a26E9081dC650EF84732\"}},{\"parameterName\":\"amount\",\"value\":{\"prototype\":\"int\",\"int\":100000000000000}}],\"payment\":{\"assetId\":300000010,\"amount\":0}}",
  "codeHash": "0x88604ca3b6d365a9bd9527be0065b6db",
  "referenceId": "example",
  "requestContext":
  {
    "userId": "0dde3a01-f675-439a-9439-0d1198ddaf8b",
    "ip": "10.244.4.0",
    "timestamp": "2021-12-10T05:51:54.1925873Z"
  }
  },
  "createdAt": "2021-12-10T05:51:54.5763207Z",
  "updatedAt": "2021-12-10T05:51:54.5763207Z"
  }
```

```protobuf
message SmartContractInvokeResponse {
  oneof result {
    SmartContractInvokeResponseBody body = 1;
    swisschain.sirius.api.common.ErrorResponseBody error = 2;
  }
}

message SmartContractInvokeResponseBody {
  SmartContractInvocation item = 1;
}

message SmartContractInvocation {
  enum SmartContractInvocationState{
    PROCESSING = 0;
    VALIDATING = 1;
    SIGNING = 2;
    SENT = 3;
    ON_CHAIN = 4;
    COMPLETED = 5;
    FAILED = 6;
    REJECTED = 7;
  }
  
  int64 id = 1;
  string tenant_id = 2;
  int64 smart_contract_id = 3;
  string smart_contract_name = 4;
  string smart_contract_address = 5;
  string blockchain_id = 6;
  string blockchain_name = 7;
  string method_name = 8;
  google.protobuf.StringValue method_address = 9;
  int64 invoker_broker_account_id = 10;
  string invoker_broker_account_name = 11;
  string invoker_broker_account_address = 12;
  int64 invoker_broker_account_details_id = 13;
  PaymentModel payment = 14;
  repeated smart_connect.data_model.Argument arguments = 15;
  SmartContractInvocationState state = 16;
  common.TransactionInfo transaction_info = 17;
  SmartContractInvocationContext invocation_context = 18;
  google.protobuf.Int64Value invocation_operation_id = 19;
  ValidatorContext validator_context = 20;
  int64 sequence = 21;
  SmartContractError error = 24;
  google.protobuf.Timestamp created_at = 22;
  google.protobuf.Timestamp updated_at = 23;
}


```

### Parameters

> POST /api/smart-contracts/{smartContractId}/invoke 200 OK
> GRPC `swisschain.sirius.api.smart_contracts.SmartContracts.Invoke`

Paginated response of the withdrawals update:

REST name | gRPC name | type | description
--------- | --------- | ---- | -------------- | -----------
`id ` | `id ` | *number* | ID of the invocation
 - | `tenant_id ` | *number* | address of the contract creator
`smartContractId ` | `smart_contract_id ` | *number* | address of the contract creator
`smartContractName ` | `smart_contract_name ` | *string* | Name of the smart contract
`smartContractAddress ` | `smart_contract_address ` | *string* | address of the smart contract
`blockchainId` | `blockchain_id` | *number* | Id of blockchain used for deployment
`blockchainName` | `blockchain_name` | *string* | Name of blockchain used for deployment
`methodName` | `method_name` | *string* | Name of method used for invocation
`methodAddress` | `method_address` | *string* | Name of method used for invocation
`brokerAccountId` | `invoker_broker_account_id` | *number* |ID of the broker account
`brokerAccountName` | `invoker_broker_account_name` | *string* | Name of the broker account
 - | `invoker_broker_account_details` | *string* | Name of the broker account
`payment ` | `payment ` | *PaymentModel* | details for payments executed during invocation
`arguments ` | `arguments ` | *repeated smart_connect.data_model.Argument* | arguments used for deployment
`state` | `state` | *string* | State of invocation
`transactionInfo` | `transaction_info` | *string* | tx details like hash and confirmations count
`invocationContext` | `invocation_context` | *string* | Details of initial invoke with document
`validatorContext` | `validator_context` | *string* | Details of validator response
`sequence` | `sequence` | *int* | update sequence
`error` | `error` | *string* | Error details if transaction failed
`createdAt` | `created_at` | *timestamp* | Datetime of invoke creation
`updatedAt` | `updated_at` | *timestamp* | Datetime of last invoke update




