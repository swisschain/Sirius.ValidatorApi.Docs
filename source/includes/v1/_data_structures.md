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

## Decision (enum)

```protobuf
package swisschain.sirius.keykeeperapi.approvalprocess.common;

enum Decision {
  DECISION_UNKNOWN = 0;
  DECISION_APPROVED = 1;
  DECISION_REJECTED = 2;
}
```

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

## ApproverResolution (object)

package swisschain.sirius.keykeeperapi.approvalrequest;

```protobuf
message ApproverResolution {
  swisschain.sirius.keykeeperapi.approvalprocess.common.Decision decision = 1;
  google.protobuf.StringValue comment = 2;
  google.protobuf.Timestamp created_at = 3;
}
```

## Custody (object)

```protobuf

package swisschain.sirius.keykeeperapi.approvalrequest;

message Custody {
  int64 id = 1;
  string name = 2;
}
```

## Subscription (object)

```protobuf

package swisschain.sirius.keykeeperapi.approvalrequest;

message Subscription {
  string name = 1;
  google.protobuf.StringValue color = 2;
}
```

## Approval request context (JSON string)

## Approval process context (JSON string)