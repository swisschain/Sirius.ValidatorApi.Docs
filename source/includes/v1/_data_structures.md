# Data structures

Here you can see data structures used by different API endpoints grouped by protobuf packages

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

## Page (object)

```protobuf
package swisschain.sirius.keykeeperapi.approvalprocess.common;

message Page {
  int32 index = 1;
  int32 size = 2;
}
```

name | type | description                                      | constraints                       | example
---- | ---- |--------------------------------------------------|-----------------------------------| -------
`index` | *in32* | Zero-based index of the page to query            | `index` >= `0`                    | `10`
`size` | *int32* | Maximum number of items to return in the results | `size` >= `0` and `size` <= `100` | `50`

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

Approval process context is a JSON string that contains approval process details specific for the type of the approval process. Thus, JSON schema depends on the *[Approval process](#data-structures-approvalprocess-object)* `type` field value. Below you can find description of the context schema for each *[Approval process type](#data-structures-approvalprocesstype-enum)*

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
	"instance": "Secure custody A",
	"ip": "234.125.135.334",
	"threshold": 3,
	"timestamp": "2022-11-25T16:22:23.832Z"
}
```

path | type | description | example
---- | ---- | ----------- | -------
`id` | *string*, *guid* | ID of the process | `44496481-4343-4d00-bdaf-29714c266a06`
`instance` | *string* | Custody instance name that initiated the process | `Secure custody A`
`ip` | *string*, *ip* | IP address of the custody component that initiated the process | `234.125.135.334`
`threshold` | *int* | Threshold indicating how many approval are need to initialize the root key | `3`
`timestamp` | *timestamp*, *ISO 8601* | When the process was started | `2022-11-25T16:22:23.832Z`

### APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION

```json
{
	"id": "44496481-4343-4d00-bdaf-29714c266a06",
	"instance": "Secure custody A",
	"ip": "234.125.135.334",
	"threshold": 3,
	"timestamp": "2022-11-25T16:22:23.832Z"
}
```

path | type | description | example
---- | ---- | ----------- | -------
`id` | *string*, *guid* | ID of the process | `44496481-4343-4d00-bdaf-29714c266a06`
`instance` | *string* | Custody instance name that initiated the process | `Secure custody A`
`ip` | *string*, *ip* | IP address of the custody component that initiated the process | `234.125.135.334`
`threshold` | *int* | Threshold indicating how many approval are need to rotate the root key | `3`
`timestamp` | *timestamp*, *ISO 8601* | When the process was started | `2022-11-25T16:22:23.832Z`

## Approval request context (JSON string)

Approval request context is a JSON string that contains approval request details specific for the type of the approval process and addressed to a certain validator. Thus, JSON schema depends on the *[Approval process](#data-structures-approvalprocess-object)* `type` field value. Below you can find description of the context schema for each *[Approval process type](#data-structures-approvalprocesstype-enum)*

### APPROVAL_PROCESS_TYPE_TRANSFER

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_DEPLOYMENT

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_INVOCATION

Not available yet

### APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION

```json
{
	"encryptedPart": "iGKvH2X6+Lr26PcPhIWK9vL+ofRS9Y/U64qgLDb2zHo356TtxqHgDXDEtI9FdwOYRTM/UOtnqV3Ol2Y7zpgGpUjXRVckmPTtwkEsmkPuPlpo1fYc6ktbSMA+K/lVRPZuYIw3x7p0hbKETtbe67gbKHgHb1CEk1Bc9/mQilLratXbVlKraSrc2bG4mBiUUzqY5f2Q/CZAWZPxcOZ5aOkrXn0adpRbQftUueouFZUWvTE=",
	"partKey": {
		"secret": "aZaG5g/GrqU1etwAzjMCtSz1vbzLNhcpBJdmwb7b6ZRyVBgEFZwMshdDZVBkygS9QfdvwgqfnHUj4QkaF6f1Ds1/+39oO0nk3J5ag5f1+/UqFFGKCQK3rekTCL5b9kzut24dD6wnz85q/XtvT7q5vq0+f5XChkFB6Bc+FiDUCoQ=",
		"nonce": "hd8XQDHjwaiht637yes6VQ=="
	},
	"responseKey": {
		"secret": "RS11UhzGJAGkMdvQ6kFPeI029WZH1y15vSfAQWQcQpCrnxkLKfMc0cB6Nh3/NpnAaad8MqCD65LoLAWQjNcPoYWuPMGOXF4LIao4eKw3K0yd8F7yEo0cswUUu7ma7/QK7BPPHZCRPGyLs8MAjCl58NijPLDJM9d6yPxl1Jr5M6U=",
		"nonce": "g0pukIMYQI9xnZg3alonDw=="
	}
}
```

path | type | description | example
---- | ---- | ----------- | -------
`encryptedPart` | *string*, *Base64* | Part of the root key encrypted by the `partKey` [AES](#api-usage-data-encryption) key | `iGKvH2X6+Lr26PcPhIWK9vL+ofRS9Y/U64qgLDb2zHo356TtxqHgDXDEtI9FdwOYRTM/UOtnqV3Ol2Y7zpgGpUjXRVckmPTtwkEsmkPuPlpo1fYc6ktbSMA+K/lVRPZuYIw3x7p0hbKETtbe67gbKHgHb1CEk1Bc9/mQilLratXbVlKraSrc2bG4mBiUUzqY5f2Q/CZAWZPxcOZ5aOkrXn0adpRbQftUueouFZUWvTE=`
`partKey` | *object* | [AES](#api-usage-data-encryption) key that `encryptedPart` is encrypted by |
`partKey.secret` | *string*, *Base64* | Secret part of the key encrypted by the [RSA-PKCS1](#api-usage-data-encryption) public key of the validator | `aZaG5g/GrqU1etwAzjMCtSz1vbzLNhcpBJdmwb7b6ZRyVBgEFZwMshdDZVBkygS9QfdvwgqfnHUj4QkaF6f1Ds1/+39oO0nk3J5ag5f1+/UqFFGKCQK3rekTCL5b9kzut24dD6wnz85q/XtvT7q5vq0+f5XChkFB6Bc+FiDUCoQ=`
`partKey.nonce` | *string*, *Base64* | Nonce part of the key | `hd8XQDHjwaiht637yes6VQ==`
`responseKey` | *object* | [AES](#api-usage-data-encryption) key that should be used to encrypt the root key part send by the validator back to the custody with the *[Approval request resolution context](#data-structures-approval-request-resolution-context-json-string)* `part` field | `RS11UhzGJAGkMdvQ6kFPeI029WZH1y15vSfAQWQcQpCrnxkLKfMc0cB6Nh3/NpnAaad8MqCD65LoLAWQjNcPoYWuPMGOXF4LIao4eKw3K0yd8F7yEo0cswUUu7ma7/QK7BPPHZCRPGyLs8MAjCl58NijPLDJM9d6yPxl1Jr5M6U=`
`responseKey.secret` | *string*, *Base64* | Secret part of the key encrypted by the [RSA-PKCS1](#api-usage-data-encryption) public key of the validator |
`responseKey.nonce` | *string*, *Base64* | Nonce part of the key | `g0pukIMYQI9xnZg3alonDw==`

### APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION

```json
{
}
```

Empty

## Approval request resolution context (JSON string)

Approval request resolution context is a JSON string that contains approval request resolution details specific for the type of the approval process formed by a certain validator. Thus, JSON schema depends on the *[Approval process](#data-structures-approvalprocess-object)* `type` field value. Below you can find description of the context schema for each *[Approval process type](#data-structures-approvalprocesstype-enum)*

### APPROVAL_PROCESS_TYPE_TRANSFER

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_DEPLOYMENT

Not available yet

### APPROVAL_PROCESS_TYPE_SMART_CONTRACT_INVOCATION

Not available yet

### APPROVAL_PROCESS_TYPE_ROOT_KEY_INITIALIZATION

```json
{
	"part": "hDXP10Ey+WKYb42hfPb3z5I4RGpRN9ODzg1j0cMPq/fSEFnwVUOH5F2TRAya0SvSzinuD29dp/8loLEcws0zRL50+LsuhDUGw4Y2Yhr0r9cSmuPeCNInBAfHUwuV8XUeK7U+KcSQqF0vKAVCF5/RgOGeJjJWxm2eiHAi7w6x8sXwr991KhrMpQwtPeYtwGGPzSRBKkyJmvzlbTdyRDL7FersvWPz0r0hQMZTGjzv0SF4d4ys0UYYkXpFYKi0kVdj1xeUivPX4pgHQjNndZgtTB6AYZRI8hLTGSK5LfYoZQUnztQc8iO3Tyn7fOiC8YypniHKAYD6p4AdCWFdZ41iwwt6Y/2CLvh6PI4FXfbLyUGnpbgyjBTEobAYh+hV+qqCTZTN9bFxLq9pAj0mdImDe1UmnlBgVH2/5BGchAbD5lWxo/1NxOseTqiqtkElMwfiQvcVj2oZG5uOO/lBmjXbhhmO33+0womgSnL3HHqOlx3bDxsQUhDdD+uPITgzDoqQg2rS1y0Gg4o79iovLzrR0A==",
	"process": {
	  "id": "44496481-4343-4d00-bdaf-29714c266a06",
	  "instance": "Secure custody A",
	  "ip": "234.125.135.334",
	  "threshold": 3,
	  "timestamp": "2022-11-25T16:22:23.832Z"
  }
}
```

path | type | description | example
---- | ---- | ----------- | -------
`part` | *string* | Root key part encrypted by the `responseKey` field of the *[Approval request context](#data-structures-approval-request-context-json-string)* | `hDXP10Ey+WKYb42hfPb3z5I4RGpRN9ODzg1j0cMPq/fSEFnwVUOH5F2TRAya0SvSzinuD29dp/8loLEcws0zRL50+LsuhDUGw4Y2Yhr0r9cSmuPeCNInBAfHUwuV8XUeK7U+KcSQqF0vKAVCF5/RgOGeJjJWxm2eiHAi7w6x8sXwr991KhrMpQwtPeYtwGGPzSRBKkyJmvzlbTdyRDL7FersvWPz0r0hQMZTGjzv0SF4d4ys0UYYkXpFYKi0kVdj1xeUivPX4pgHQjNndZgtTB6AYZRI8hLTGSK5LfYoZQUnztQc8iO3Tyn7fOiC8YypniHKAYD6p4AdCWFdZ41iwwt6Y/2CLvh6PI4FXfbLyUGnpbgyjBTEobAYh+hV+qqCTZTN9bFxLq9pAj0mdImDe1UmnlBgVH2/5BGchAbD5lWxo/1NxOseTqiqtkElMwfiQvcVj2oZG5uOO/lBmjXbhhmO33+0womgSnL3HHqOlx3bDxsQUhDdD+uPITgzDoqQg2rS1y0Gg4o79iovLzrR0A==`
`process` | *object* | *[Approval process context](#data-structures-approval-process-context-json-string)* to which the resolution is related to. Just pass all process fields you've received in the *[Approval process context](#data-structures-approval-process-context-json-string)* |
`process.id` | *string*, *guid* | ID of the process | `44496481-4343-4d00-bdaf-29714c266a06`
`process.instance` | *string* | Custody instance name that initiated the process | `Secure custody A`
`process.ip` | *string*, *ip* | IP address of the custody component that initiated the process | `234.125.135.334`
`process.threshold` | *int* | Threshold indicating how many approval are need to initialize the root key | `3`
`process.timestamp` | *timestamp*, *ISO 8601* | When the process was started | `2022-11-25T16:22:23.832Z`

### APPROVAL_PROCESS_TYPE_ROOT_KEY_ROTATION

```json
{
	"process": {
	  "id": "44496481-4343-4d00-bdaf-29714c266a06",
	  "instance": "Secure custody A",
	  "ip": "234.125.135.334",
	  "threshold": 3,
	  "timestamp": "2022-11-25T16:22:23.832Z"
  }
}
```

path | type | description | example
---- | ---- | ----------- | -------
`process` | *object* | *[Approval process context](#data-structures-approval-process-context-json-string)* to which the resolution is related to. Just pass all process fields you've received in the *[Approval process context](#data-structures-approval-process-context-json-string)* |
`process.id` | *string*, *guid* | ID of the process | `44496481-4343-4d00-bdaf-29714c266a06`
`process.instance` | *string* | Custody instance name that initiated the process | `Secure custody A`
`process.ip` | *string*, *ip* | IP address of the custody component that initiated the process | `234.125.135.334`
`process.threshold` | *int* | Threshold indicating how many approval are need to rotate the root key | `3`
`process.timestamp` | *timestamp*, *ISO 8601* | When the process was started | `2022-11-25T16:22:23.832Z`
