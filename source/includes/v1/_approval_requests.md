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

### Response

## Get approval request details

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Get`

`Authorization required`

Gets detailed information about a specified approval request. This method is intended to be invoked only for a single approval request, do not
use it for bluk infromation retrival. If you need to get information about many approval requests, consider using *[Get list of approval requests](#approval-requests-get-list-of-approval-requests)*.

### Request

### Response

## Acknowledge an approval request is received

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Acknowledge`

`Authorization required`

Marks a specified approval request as received by the validator application.

### Request

### Response

## Notify an approval request is read

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Read`

`Authorization required`

Marks a specified approval request as read by the validation officer.

### Request

### Response

## Approve an approval request

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Approve`

`Authorization required`

Approves a specified approval request.

### Request

### Response

## Reject an approval request

`swisschain.sirius.keykeeperapi.approvalrequest.ApprovalRequests/Reject`

`Authorization required`

Rejects a specified approval request.

### Request

### Response