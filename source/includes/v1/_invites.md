# Invites

Invitation allows to register a validator on a Universe Subscription.
Inivitation can be generated on the Universe portal. 

Before a validator can accept an invitation it needs to generate its
keys pair and validator ID. Keys pairs is an `RSA-PKCS1` keys with the following parameters:

- Public exponent: `3`
- Strength: `1024`
- Certainty: `25`

Validator ID is a `SHA256Digest` hash of the validator public key.

The private key should be kept in secret by the validator. The public key and validator ID
should be submitted to the *[Accept](#invites-accept-an-invitation)* method of API.

## Accept an invitation

Accepts a validator invitation generated via Universe portal.

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

name | type | placement | description 
---- | ---- | ----------| -----------
`invite_id` | *string* | body | Invitation ID to accept
`validator_id` | *string* | body | ID of the validator
`public_key` | *string* | body | Public key of the validator

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

#### AcceptInviteResponseBody

name | type | description
-----| ---- | -----------
`name` | *string* | Name of the validator specified on the Universe portal
`position` | *optional*, *string* | Position of the validator specified on the Universe portal
`description` | *optional*, *string* | Description of the validator specified on the Universe portal
`api_key` | *string* | API key of the validator to be used for all further API requests

## Revoke an invitation

***Authorization required***

Revokes a previously accepted invitation. 

### Request

`swisschain.sirius.validator_api.invites.Invites.Revoke`

```protobuf
swisschain.sirius.validator_api.invites.Invites.Revoke

> Requets: (application/grpc)

message RevokeInviteRequest {
}
```

name | type | placement | description
---- | ---- | --------- | -----------
`Bearer` | *string* | metadata | API key of the validator returned by the *[Accept](#invites-accept-an-invitation)* method

### Response

```protobuf
swisschain.sirius.validator_api.invites.Invites.Revoke

> Response: (application/grpc) - success response

message RevokeInviteResponse {
    oneof body {
        AcceptInviteResponseBody response = 1;
        .swisschain.sirius.validator_api.common.Error error = 2;
    }
}

message RevokeInviteResponseBody {
}
```

N/A