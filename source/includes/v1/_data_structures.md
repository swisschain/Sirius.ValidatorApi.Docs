# Data structures

Here you can see data structures used by different API endpoints groupped by protobuf packages

## Error (object)

```protobuf
package swisschain.sirius.validator_api.common;

message Error {
  ErrorCode code = 1;
  string message = 2;
}
```

name | type | description | example
---- | ---- | ----------- | -------
`code` | *[ErrorCode](#data-structures-errorcode-enum)* | Error code |
`message` | *string* | Error message | `API key is expired`

## ErrorCode (enum)

```protobuf
package swisschain.sirius.validator_api.common;

enum ErrorCode {
  UNKNOWN = 0;
  INVALID_PARAMETERS = 1;
  NOT_FOUND = 2;
  EXPIRED_API_KEY = 3;
  INVALID_API_KEY = 4;
}
```

name | value | description
---- | ----- | -----------
`UNKNOWN` | `0` | Transient error or program error
`INVALID_PARAMETERS` | `1` | Invalid request parameters
`NOT_FOUND` | `2` | Object not found
`EXPIRED_API_KEY` | `3` | API key is expired
`INVALID_API_KEY` | `4` | API key is invalid

## ApprovalProcessType (enum)

```protobuf
package swisschain.sirius.keykeeperapi.approvalprocess.common;

enum ApprovalProcessType {
  APPROVAL_PROCESS_TYPE_UNKNOWN = 0;
  APPROVAL_PROCESS_TYPE_TRANSFER = 1;
  APPROVAL_PROCESS_TYPE_SMART_CONTRACT_DEPLOYMENT = 2;
  APPROVAL_PROCESS_TYPE_SMART_CONTRACT_INVOCATION = 3;
  APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION = 4;
  APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION = 5;
}
```

name | value | description
---- | ----- | -----------
`APPROVAL_PROCESS_TYPE_UNKNOWN` | `0` | Unexpected value. This should not be returned normally
`APPROVAL_PROCESS_TYPE_TRANSFER` | `1` | A transfer blockchain transaction
`APPROVAL_PROCESS_TYPE_SMART_CONTRACT_DEPLOYMENT` | `2` | A smart contract deployment blockchain transaction
`APPROVAL_PROCESS_TYPE_SMART_CONTRACT_INVOCATION` | `3` | A smart contract method invocation blockchain transaction
`APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION` | `4` | A custody root key initialization
`APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION` | `5` | A custody root key rotation

## ApprovalProcessStatus (enum)

```protobuf
package swisschain.sirius.keykeeperapi.approvalprocess.common;

enum ApprovalProcessStatus {
  APPROVAL_PROCESS_STATUS_UNKNOWN = 0;
  APPROVAL_PROCESS_STATUS_PENDING = 1;
  APPROVAL_PROCESS_STATUS_APPROVED = 2;
  APPROVAL_PROCESS_STATUS_REJECTED = 3;
}
```

name | value | description
---- | ----- | -----------
`APPROVAL_PROCESS_STATUS_UNKNOWN` | `0` | Unexpected value. This should not be returned normally
`APPROVAL_PROCESS_STATUS_PENDING` | `1` | An approval process is pending for resolutions
`APPROVAL_PROCESS_STATUS_APPROVED` | `2` | An approval process is approved
`APPROVAL_PROCESS_STATUS_REJECTED` | `3` | An approval process is rejected

## Decision (enum)

```protobuf
package swisschain.sirius.keykeeperapi.approvalprocess.common;

enum Decision {
  DECISION_UNKNOWN = 0;
  DECISION_APPROVED = 1;
  DECISION_REJECTED = 2;
}
```

name | value | description
---- | ----- | -----------
`DECISION_UNKNOWN` | `0` | Unexpected value. This should not be returned normally
`DECISION_APPROVED` | `1` | A validator approved a validation request
`DECISION_REJECTED` | `2` | A validator rejected a validation request

## ApprovalProcess (object)

```protobuf
package swisschain.sirius.keykeeperapi.approvalrequest;

message ApprovalProcess {
  string id = 1;
  swisschain.sirius.keykeeperapi.approvalprocess.common.ApprovalProcessType type = 2;
  swisschain.sirius.keykeeperapi.approvalprocess.common.ApprovalProcessStatus status = 3;
  string context = 4;
}
```

name | type | description | example
---- | ---- | ----------- | -------
`id` | *string*, *guid* | ID of the approval process | `52635969-3c80-422a-bafc-7e5a6151bf85`
`type` | *[ApprovalProcessType](#data-structures-approvalprocesstype-enum)* | Type of the approval process | `APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION`
`status` | *[ApprovalProcessStatus](#data-structures-approvalprocessstatus-enum) | Status of the approval process | `APPROVAL_PROCESS_STATUS_PENDING`
`context` | *string*, *JSON* | Approval process context. It is a JSON string. Schema of JSON depends on the approval process `type` field value

## ApproverResolution (object)

```protobuf
package swisschain.sirius.keykeeperapi.approvalrequest;

message ApproverResolution {
  swisschain.sirius.keykeeperapi.approvalprocess.common.Decision decision = 1;
  google.protobuf.StringValue comment = 2;
  google.protobuf.Timestamp created_at = 3;
}
```

name | type | description | example
---- | ---- | ----------- | -------
`decision` | *[Decision](#data-structures-decision-enum)* | Decision made by the approver |
`comment` | *optional*, *string* | Comment made by the approver | `This transaction is not allowed`
`created_at` | *timestamp* | When the approver resolution was made | `2022-11-25T15:26:43.185` 

## Custody (object)

```protobuf
package swisschain.sirius.keykeeperapi.approvalrequest;

message Custody {
  int64 id = 1;
  string name = 2;
}
```

name | type | description | example
---- | ---- | ----------- | -------
`id` | *int64* | ID of the custody | `404000019`
`name` | *string* | Name of the custody | `Treasury custody`

## Subscription (object)

```protobuf
package swisschain.sirius.keykeeperapi.approvalrequest;

message Subscription {
  string name = 1;
  google.protobuf.StringValue color = 2;
}
```
name | type | description | example
---- | ---- | ----------- | -------
`name` | *string* | Name of the subscription | `Sandbox`
`color` | *optional*, *string* | Color of associated with the subscription on the Universe portal | `#ff0000`

## Approval process context (JSON string)

Approval process context is a JSON string that contains approval process details specific for the type of the approval process. Thus JSON schema depends on the *[Approval process](#data-structures-approvalprocess-object)* `type` field value. Below you can find description of the context schema for each *[Approval process type](#data-structures-approvalprocesstype-enum)*

### APPROVAL_PROCESS_TYPE_TRANSFER

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_DEPLOYMENT

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_INVOCATION

Not available yet

### APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION

```json
{
	"id": "44496481-4343-4d00-bdaf-29714c266a06",
	"instance": "TODO: get an example",
	"ip": "234.125.135.334",
	"threshold": 3,
	"timestamp": "2022-11-25T16:22:23.832Z"
}
```

### APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION

```json
{
	"id": "44496481-4343-4d00-bdaf-29714c266a06",
	"instance": "TODO: get an example",
	"ip": "234.125.135.334",
	"threshold": 3,
	"timestamp": "2022-11-25T16:22:23.832Z"
}
```

## Approval request context (JSON string)

Approval request context is a JSON string that contains approval request details specific for the type of the approval process and addressed to a certain validator. Thus JSON schema depends on the *[Approval process](#data-structures-approvalprocess-object)* `type` field value. Below you can find description of the context schema for each *[Approval process type](#data-structures-approvalprocesstype-enum)*

### APPROVAL_PROCESS_TYPE_TRANSFER

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_DEPLOYMENT

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_INVOCATION

Not available yet

### APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION

```json
{
	"encryptedPart": "string",
	"partKey": { // root key part AES encryption key
		"secret": "key", // encrypted by approver public key
		"nonce": "string"
	},
	"responseKey": { // used to safetly transfer restored root key part to vault
		"secret": "key", // encrypted by approver public key
		"nonce": "string"
	}
}
```

### APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION

```json
{
}
```

## Approval request resolution context (JSON string)

Approval request resolution context is a JSON string that contains approval request resolution details specific for the type of the approval process formed by a certain validator. Thus JSON schema depends on the *[Approval process](#data-structures-approvalprocess-object)* `type` field value. Below you can find description of the context schema for each *[Approval process type](#data-structures-approvalprocesstype-enum)*

### APPROVAL_PROCESS_TYPE_TRANSFER

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_DEPLOYMENT

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_INVOCATION

Not available yet

### APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION

```json
{
	"part": "string", // restored root key part encrypted by response key
	"process": "root-key-initialization-process-context.json"
}
```

### APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION

```json
{
	"process": "root-key-rotation-process-context.json"
}
```