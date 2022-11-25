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

`swisschain.sirius.validator_api.invites.Invites/Accept`

Accepts a validator invitation generated via Universe portal.

### Request

```protobuf
swisschain.sirius.validator_api.invites.Invites/Accept

> Requets: (application/grpc)

message AcceptInviteRequest {
  string invite_id = 1;
  string validator_id = 2;
  string public_key = 3;
}
```

name | type | description | example
---- | ---- | ----------- | -------
`invite_id` | *string* | Invitation ID to accept | `3d44bfc617164889b21928ac2c256ffcdead0e33918f4f27a333c2025b8f4447b6b313241ebc4a0e8133c7479337f9ed`
`validator_id` | *string* | ID of the validator. `Base64` encoded string of the `SHA256Digest` hash of the `public_key` | `iPC78NoD2KpFFig8FHR7Pg403d+FCKwMorjaEBXn5PY=`
`public_key` | *string* | Single-line `RSA-PKCS1` public key of the validator | `-----BEGIN PUBLIC KEY-----`<br>`MIGdMA0GCSqGSIb3DQEBAQUAA4GLADCBhwKBgQCQN1JrAyX/FsP3v4vmE9aA/95N7EpwKSDujcsJRwttVlT803vU8DSsLAGDAlnb0YqeEhYaaDaGTTeOERitvt9QMnOoLwCuX7Fncp0RYclktjb9yLOl6zxEM5g57bVlCqv78AFfxKwHpt535hMg/bKG1rrNZR9NHh0GACdgsuV8GQIBAw==`<br>`-----END PUBLIC KEY-----`

### Response

```protobuf
swisschain.sirius.validator_api.invites.Invites/Accept

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

#### AcceptInviteResponse

name | type | description
---- | ---- | -------
`response`| *oneof body*, *[AcceptInviteResponseBody](#invites-accept-an-invitation-response-acceptinviteresponsebody-object)* | Response payload
`error` | *oneof body*, *[Error](#data-structures-error-object)* | Response error

#### AcceptInviteResponseBody

name | type | description | example
-----| ---- | ----------- | -------
`name` | *string* | Name of the validator specified on the Universe portal | `Jhon Doe`
`position` | *optional*, *string* | Position of the validator specified on the Universe portal | `Risk manager`
`description` | *optional*, *string* | Description of the validator specified on the Universe portal | `Very important guy`
`api_key` | *string* | API key of the validator to be used to authenticate further API requests | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ2YWxpZGF0b3ItaWQiOiI3MDEwMDAwMTUiLCJuYmYiOjE2NjcyNDQyNzEsImV4cCI6MTY5ODc4MDI3MSwiaWF0IjoxNjY3MjQ0MjcxLCJhdWQiOiJzaXJpdXMuc3dpc3NjaGFpbi5pbyJ9.QkhXNhb3EVyoO7KRb2jiWDQV0gWjASCyhMXsPl5i9g8`

## Revoke an invitation

`swisschain.sirius.validator_api.invites.Invites/Revoke`

`Authorization required`

Revokes a previously accepted invitation. 

### Request

```protobuf
swisschain.sirius.validator_api.invites.Invites/Revoke

> Requets: (application/grpc)

message RevokeInviteRequest {
}
```

Empty

### Response

```protobuf
swisschain.sirius.validator_api.invites.Invites/Revoke

> Response: (application/grpc) - success response

message RevokeInviteResponse {
    oneof body {
        RevokeInviteResponseBody response = 1;
        .swisschain.sirius.validator_api.common.Error error = 2;
    }
}

message RevokeInviteResponseBody {
}
```

#### RevokeInviteResponse

name | type | description
---- | ---- | -------
`response`| *oneof body*, *[RevokeInviteResponseBody](#invites-accept-an-invitation-response-revokeinviteresponsebody-object)* | Response payload
`error` | *oneof body*, *[Error](#data-structures-error-object)* | Response error

#### RevokeInviteResponseBody

Empty