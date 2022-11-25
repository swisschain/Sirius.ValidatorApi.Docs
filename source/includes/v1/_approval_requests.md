# Approval requests

Approval requests is what this API all about. Whenever a custody needs an approval from validators it initiates an approval request.
Each approval request object represents an overall process of approving of something. This object is a general one and it fits all the different
approval requests like root key initialization, transfer transaction, smart contract invocation and so on. This is achieved by having a `string context`
field that contains a approva process type-specific data serialized as a JSON object. Each validator sees its onw representation of an
approval request where he sees its own approval status and approval statuses of all other validators separately. Once an approval request
is approved or rejected it can not become a pending again.

## Get list of approval requests

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/List`

`Authorization required`

Gets list of approval requests related to the current (authenticated) validator with pagination and filtering.
This endpoint returns brief information about approval requests. To get detailed approval request information see *[Get approval request details](#approval-requests-get-approval-request-details)*.

### Request

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/List

> Requets: (application/grpc)

message ListApprovalRequestsRequest {
  Page page = 1;
  ListApprovalRequestsFilter filter = 2;  
}

message Page {
  int32 index = 1;
  int32 size = 2;
}

message ListApprovalRequestsFilter {
  google.protobuf.BoolValue is_read = 1;
  google.protobuf.Int64Value custody_id = 2;
  google.protobuf.BoolValue is_processed = 3;
}
```

#### ListApprovalRequestsRequest (object)

name | type | description
---- | ---- | ----------- 
`page` | *[Page](#approval-requests-get-list-of-approval-requests-request-page-object)* | page selector
`filter` | *[ListApprovalRequestsFilter](#approval-requests-get-list-of-approval-requests-request-listapprovalrequestsfilter-object)* | filter parameters

#### Page (object)

name | type | description | contraints | example
---- | ---- | ----------- | ---------- | -------
`index` | *in32* | Zero-based index of the page to query | `index` >= `0` | `10`
`size` | *int32* | Maximum number of items to return in the results | `size` >= `0` and `size` <= `100` | `50`

#### ListApprovalRequestsFilter (object)

name | type | description | example
---- | ---- | ----------- | -------
`is_read` | *bool?* | If specified, indicates if API should return only read/not read (by the given validator) approval requests | `<null>`, `true`, `false`
`custody_id` | *int64?* | If specified, indicates API to return only approval requests related to the specified custody | `<null>`, `404000100`
`is_processed` | *bool?* | If specified, indicates if API should return only processed/not processed approval requests. Processed means that an approval request is either approved or rejected |  `<null>`, `true`, `false`

### Response

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/List

> Response: (application/grpc) - success response

message ListApprovalRequestsResponse {
  oneof body {
    ListApprovalRequestsPayload payload = 1;
    ListApprovalRequestsError error = 2;
  }
}

message ListApprovalRequestsPayload {
  repeated ApprovalRequestListItem items = 1;
}

message ApprovalRequestListItem {
  int64 id = 1;
  ApprovalProcess approval_process = 2;
  string context = 3;
  Custody custody = 4;
  Subscription subscription = 5;
  ApproverResolution approver_resolution = 6;
  bool is_read = 7;
  google.protobuf.Timestamp read_at = 8;
  google.protobuf.Timestamp delivered_at = 9;
  google.protobuf.Timestamp created_at = 10;
}

message ListApprovalRequestsError {

  enum ErrorCode {
    UNKNOWN = 0;
    INVALID_PARAMETERS = 1;
    DOMAIN_PROBLEM = 2;
    TECHNICAL_PROBLEM = 3;
    UNAUTHORIZED = 4;
  }

  ErrorCode code = 1;
  string message = 2;
}
```

#### ListApprovalRequestsResponse (object)

name | type | description
---- | ---- | -------
`payload`| *oneof body*, *[ListApprovalRequestsPayload](#approval-requests-get-list-of-approval-requests-response-listapprovalrequestspayload-object)* | Response payload
`error` | *oneof body*, *[ListApprovalRequestsError](#approval-requests-get-list-of-approval-requests-response-listapprovalrequestserror-object)* | Response error

#### ListApprovalRequestsPayload (object)

name | type | description
---- | ----------- | -------
`items` | *[ApprovalRequestListItem](#approval-requests-get-list-of-approval-requests-response-approvalrequestlistitem-object)[]* | Approval request items


#### ApprovalRequestListItem (object)

name | type | description | example
---- | ---- | ----------- | -------
`id` | *int64* | ID of the approval request | `702000453`
`approval_process` | *[ApprovalProcess](#data-structures-approvalprocess-object)* | Details of the approval process to which the approval request is related to |
`context` | *string*, *JSON* | Approval request context. It is a JSON string. Schema of JSON depends on the approval process type (see `approval_process.type`) |
`custody` | *[Custody](#data-structures-custody-object)* | Details of the custody to which the approval request is related to |
`subscription` | *[Subscription](#data-structures-subscription-object)* | Details of the Universe portal subscription to which the approval request is related to |
`approver_resolution` | *optional*, *[ApproverResolution](#data-structures-approverresolution-object)* | Details of the approver resolution related to the approval request. Can be `null` |
`is_read` | *bool* | Indicates if the approval request was read by the validator (if *[Read](#approval-requests-notify-an-approval-request-is-read)* endpoint was invoked) | `true`, `false`
`read_at` | *optional*, *timestamp* | Timestamp when the approval request was read by the validator (when *[Read](#approval-requests-notify-an-approval-request-is-read)* endpoint was invoked) | `<null>`, `2022-11-25T11:49:03.456Z`
`delivered_at` | *optional*, *timestamp* | Timestamp when the approval request was delivered to the validator (when *[Acknowledge](#approval-requests-acknowledge-an-approval-request-is-received)* endpoint was invoked) | `<null>`, `2022-11-25T11:49:03.456Z`
`created_at` | *timestamp* | Timestamp when the approval request was created | `2022-11-25T11:49:03.456Z`

#### ListApprovalRequestsError (object)

name | type | description | example
---- | ---- | ----------- | -------
`code` | *[ListApprovalRequestsError.ErrorCode](#approval-requests-get-list-of-approval-requests-response-listapprovalrequestserror-errorcode-enum)* | Error code | `INVALID_PARAMETERS`
`message` | *string* | Error message | `Page size should be a positive number`

#### ListApprovalRequestsError.ErrorCode (enum)

name | value | description
---- | ----- | -----------
`UNKNOWN` | `0` | An unexpected error. This should be returned normally
`INVALID_PARAMETERS` | `1` | Invalid request parameters
`DOMAIN_PROBLEM` | `2` | Operation can't be performed with the current server state and with the specified parameters 
`TECHNICAL_PROBLEM` | `3` | A transient error or a program error
`UNAUTHORIZED` | `4` | Unauthorized request

## Get approval request details

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Get`

`Authorization required`

Gets detailed information about a specified approval request. This method is intended to be invoked only for a single approval request, do not
use it for bluk infromation retrival. If you need to get information about many approval requests, consider using *[Get list of approval requests](#approval-requests-get-list-of-approval-requests)*.

### Request

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Get

> Requets: (application/grpc)

message GetApprovalRequestRequest {
  int64 id = 1;
}
```

name | type | description | example
---- | ---- | ----------- | -------
`id` | int64 | Id of an approval request to get details | `702000453`

### Response

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Get

> Response: (application/grpc) - success response

message GetApprovalRequestResponse {
  oneof body {
    GetApprovalRequestPayload payload = 1;
    GetApprovalRequestError error = 2;
  }
}

message GetApprovalRequestPayload {
  int64 id = 1;
  ApprovalProcess approval_process = 2;
  string context = 3;
  Custody custody = 4;
  Subscription subscription = 5;
  ApproverResolution approver_resolution = 6;
  repeated LinkedApprovalRequest linked_approval_requests = 7;
  bool is_read = 8;
  google.protobuf.Timestamp read_at = 9;
  google.protobuf.Timestamp delivered_at = 10;
  google.protobuf.Timestamp created_at = 11;
}

message GetApprovalRequestError {

  enum ErrorCode {
    UNKNOWN = 0;
    INVALID_PARAMETERS = 1;
    DOMAIN_PROBLEM = 2;
    TECHNICAL_PROBLEM = 3;
    UNAUTHORIZED = 4;
  }

  ErrorCode code = 1;
  string message = 2;
}

message LinkedApprovalRequest {
  Approver approver = 1;
  ApproverResolution approver_resolution = 2;
  bool is_read = 3;
  bool is_delivered = 4;
  google.protobuf.Timestamp read_at = 5;
  google.protobuf.Timestamp delivered_at = 6;
}

message Approver {
  string id = 1;
  string name = 2;
}
```

#### GetApprovalRequestResponse (object)

name | type | description
---- | ---- | -------
`payload`| *oneof body*, *[GetApprovalRequestPayload](#approval-requests-get-list-of-approval-requests-response-getapprovalrequestpayload-object)* | Response payload
`error` | *oneof body*, *[GetApprovalRequestError](#approval-requests-get-list-of-approval-requests-response-getapprovalrequesterror-object)* | Response error

#### GetApprovalRequestPayload (object)

name | type | description | example
---- | ---- | ----------- | -------
`id` | *int64* | ID of the approval request | `702000453`
`approval_process` | *[ApprovalProcess](#data-structures-approvalprocess-object)* | Details of the approval process to which the approval request is related to |
`context` | *string*, *JSON* | Approval request context. It is a JSON string. Schema of JSON depends on the approval process type (see `approval_process.type`) |
`custody` | *[Custody](#data-structures-custody-object)* | Details of the custody to which the approval request is related to |
`subscription` | *[Subscription](#data-structures-subscription-object)* | Details of the Universe portal subscription to which the approval request is related to |
`linked_approval_requests` | *[LinkedApprovalRequest](#approval-requests-get-approval-request-details-response-linkedapprovalrequest-object)[]* | List of approval requests send to other validators within given approval process |
`approver_resolution` | *optional*, *[ApproverResolution](#data-structures-approverresolution-object)* | Details of the approver resolution related to the approval request. Can be `null` |
`is_read` | *bool* | Indicates if the approval request was read by the validator (if *[Read](#approval-requests-notify-an-approval-request-is-read)* endpoint was invoked) | `true`, `false`
`read_at` | *optional*, *timestamp* | Timestamp when the approval request was read by the validator (when *[Read](#approval-requests-notify-an-approval-request-is-read)* endpoint was invoked) | `<null>`, `2022-11-25T11:49:03.456Z`
`delivered_at` | *optional*, *timestamp* | Timestamp when the approval request was delivered to the validator (when *[Acknowledge](#approval-requests-acknowledge-an-approval-request-is-received)* endpoint was invoked) | `<null>`, `2022-11-25T11:49:03.456Z`
`created_at` | *timestamp* | Timestamp when the approval request was created | `2022-11-25T11:49:03.456Z`

#### LinkedApprovalRequest (object)

name | type | description | example
---- | ---- | ----------- | -------
`approver` | Details of the validator to which given approval request is related to | 
`approver_resolution` | *optional*, *[ApproverResolution](#data-structures-approverresolution-object)* | Details of the approver resolution related to the approval request. Can be `null` |
`is_read` | *bool* | Indicates if the approval request was read by the validator (if *[Read](#approval-requests-notify-an-approval-request-is-read)* endpoint was invoked) | `true`, `false`
`is_delivered` | *bool* | Indicates if the approval request was delivered to the validator (if *[Acknowledge](#approval-requests-acknowledge-an-approval-request-is-received)* endpoint was invoked) | `true`, `false`
`read_at` | *optional*, *timestamp* | Timestamp when the approval request was read by the validator (when *[Read](#approval-requests-notify-an-approval-request-is-read)* endpoint was invoked) | `<null>`, `2022-11-25T11:49:03.456Z`
`delivered_at` | *optional*, *timestamp* | Timestamp when the approval request was delivered to the validator (when *[Acknowledge](#approval-requests-acknowledge-an-approval-request-is-received)* endpoint was invoked) | `<null>`, `2022-11-25T11:49:03.456Z`

#### Approver (object)

name | type | description | example
---- | ---- | ----------- | -------
`id` | *string* | ID of the approver | `iPC78NoD2KpFFig8FHR7Pg403d+FCKwMorjaEBXn5PY=`
`name` | *string* | Name of the approver | `Jhon Doe`

#### GetApprovalRequestError (object)

name | type | description | example
---- | ---- | ----------- | -------
`code` | *[GetApprovalRequestError.ErrorCode](#approval-requests-get-approval-request-details-response-getapprovalrequesterror-errorcode-enum)* | Error code | `DOMAIN_PROBLEM`
`message` | *string* | Error message | `Approval request not found.`

#### GetApprovalRequestError.ErrorCode (enum)

name | value | description
---- | ----- | -----------
`UNKNOWN` | `0` | An unexpected error. This should be returned normally
`INVALID_PARAMETERS` | `1` | Invalid request parameters
`DOMAIN_PROBLEM` | `2` | Operation can't be performed with the current server state and with the specified parameters 
`TECHNICAL_PROBLEM` | `3` | A transient error or a program error
`UNAUTHORIZED` | `4` | Unauthorized request

## Acknowledge an approval request is received

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Acknowledge`

`Authorization required`

Marks a specified approval request as received by the validator application.

### Request

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Acknowledge

> Requets: (application/grpc)

message AcknowledgeApprovalRequestRequest {
  int64 id = 1;
}
```

### Response

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Acknowledge

> Response: (application/grpc) - success response

message AcknowledgeApprovalRequestResponse {
  oneof body {
    AcknowledgeApprovalRequestPayload payload = 1;
    AcknowledgeApprovalRequestError error = 2;
  }
}

message AcknowledgeApprovalRequestPayload {
}

message AcknowledgeApprovalRequestError {

  enum ErrorCode {
    UNKNOWN = 0;
    INVALID_PARAMETERS = 1;
    DOMAIN_PROBLEM = 2;
    TECHNICAL_PROBLEM = 3;
    UNAUTHORIZED = 4;
  }

  ErrorCode code = 1;
  string message = 2;
}
```

## Notify an approval request is read

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Read`

`Authorization required`

Marks a specified approval request as read by the validation officer.

### Request

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Read

> Requets: (application/grpc)

message ReadApprovalRequestRequest {
  int64 id = 1;
}
```

### Response

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Read

> Response: (application/grpc) - success response

message ReadApprovalRequestResponse {
  oneof body {
    ReadApprovalRequestPayload payload = 1;
    ReadApprovalRequestError error = 2;
  }
}

message ReadApprovalRequestPayload {
}

message ReadApprovalRequestError {

  enum ErrorCode {
    UNKNOWN = 0;
    INVALID_PARAMETERS = 1;
    DOMAIN_PROBLEM = 2;
    TECHNICAL_PROBLEM = 3;
    UNAUTHORIZED = 4;
  }

  ErrorCode code = 1;
  string message = 2;
}
```

## Approve an approval request

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Approve`

`Authorization required`

Approves a specified approval request.

### Request

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Approve

> Requets: (application/grpc)

message ApproveApprovalRequestRequest {
  int64 id = 1;
  string comment = 2;
  string context = 3;
  string signature = 4;
  string device_info = 5;
  string app_version = 6;
}
```

### Response

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Approve

> Response: (application/grpc) - success response

message ApproveApprovalRequestResponse {
  oneof body {
    ApproveApprovalRequestPayload payload = 1;
    ApproveApprovalRequestError error = 2;
  }
}

message ApproveApprovalRequestPayload {
}

message ApproveApprovalRequestError {

  enum ErrorCode {
    UNKNOWN = 0;
    INVALID_PARAMETERS = 1;
    DOMAIN_PROBLEM = 2;
    TECHNICAL_PROBLEM = 3;
    UNAUTHORIZED = 4;
  }

  ErrorCode code = 1;
  string message = 2;
}
```

## Reject an approval request

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Reject`

`Authorization required`

Rejects a specified approval request.

### Request

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Reject

> Requets: (application/grpc)

message RejectApprovalRequestRequest {
  int64 id = 1;
  string comment = 2;
  string context = 3;
  string signature = 4;
  string device_info = 5;
  string app_version = 6;
}
```

### Response

```protobuf
swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Reject

> Response: (application/grpc) - success response

message RejectApprovalRequestResponse {
  oneof body {
    RejectApprovalRequestPayload payload = 1;
    RejectApprovalRequestError error = 2;
  }
}

message RejectApprovalRequestPayload {
}

message RejectApprovalRequestError {

  enum ErrorCode {
    UNKNOWN = 0;
    INVALID_PARAMETERS = 1;
    DOMAIN_PROBLEM = 2;
    TECHNICAL_PROBLEM = 3;
    UNAUTHORIZED = 4;
  }

  ErrorCode code = 1;
  string message = 2;
}
```