# Invites

## Accept an invitation

### Request

`swisschain.sirius.validator_api.invites.Invites.Accept`

```protobuf
swisschain.sirius.validator_api.invites.Invites.Accept

> Requets: (application/grpc)

message AcceptInviteRequest {
  string invite_id = 1;
  string validator_id = 2;
  string public_key = 3;
}
```

name | type | description 
---- | ---- | -------------- | -----------
`invite_id` | *string* | Invitation ID to accept
`validator_id` | *string* | ID of the validator
`public_key` | *string* | Public key of the validator

### Response

```protobuf
swisschain.sirius.validator_api.invites.Invites.Accept

> Response: (application/grpc) - success response

message AcceptInviteResponse {
    oneof body {
        AcceptInviteResponseBody response = 1;
        .swisschain.sirius.validator_api.common.Error error = 2;
    }
}

message AcceptInviteResponseBody {
    string name = 1;
    string position = 2;
    string description = 3;
    string api_key = 4;
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

## Revoke an invitation

### Request

`swisschain.sirius.api.accounts.Accounts.Update`

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