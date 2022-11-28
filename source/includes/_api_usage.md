# API usage

Sirius Validator API is a gRPC API. To get proto files please follow [our GitHub](https://github.com/swisschain/Sirius.ValidatorApi.Docs/tree/master/.proto)

To learn more about gRPC please follow [grpc.io](https://grpc.io)

## Authentication

Sirius Validator API uses `Bearer authentication`. Authorization token that a client needs to send with a request is a JWT token.
The token can be obtained during the Validator registration process that can be initiated via [Universe portal](https://universe.swisschain.io).
See *[Invites](#invites)* section to get more details.

All API endpoints marked with

`Authorization required`

require authorization token to be passed as a gRPC metadata item with the key `Bearer`:

name | type | placement | description | example
---- | ---- | --------- | ----------- | -------
`Bearer` | *string* | metadata | API key of the validator returned by the *[Accept](#invites-accept-an-invitation)* method | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ2YWxpZGF0b3ItaWQiOiI3MDEwMDAwMTUiLCJuYmYiOjE2NjcyNDQyNzEsImV4cCI6MTY5ODc4MDI3MSwiaWF0IjoxNjY3MjQ0MjcxLCJhdWQiOiJzaXJpdXMuc3dpc3NjaGFpbi5pbyJ9.QkhXNhb3EVyoO7KRb2jiWDQV0gWjASCyhMXsPl5i9g8`

For simplicity this metadata item is not shown in every request that requires authorization.

## Guid

Some API objects have string fields that in fact contain [Guid](http://guid.one/guid). Exact format is `xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx`

## JSON string

Although given API is a gRPC API, some gRPC objects may have string fields that contain some data serialized in the JSON format. Mainly such JSON strings are used
to represent data that should be signed. Data can be serialized to JSON using any serialization options, the only requirement - JSON should follow [RFC 8259](https://datatracker.ietf.org/doc/rfc8259)

## JSON string timestamp

Since timestamp representation is not fixed in the RFC 8259, given API uses textual representation following [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Exact format is `yyyy-MM-ddThh:mm:ss.ffffffZ`. Timestamps are always in UTC time zone

## Data encryption

Some data is passed in encrypted form. For symmetrical encryption `AES` algorythm is used. For assymetrical encryption `RSA-PKCS1` algorythm is used.

Parameters of `AES` algorythm are:

* Key bits size: `256`
* Nonce bits size: `128`

Parameters of `RSA-PKCS1` algorythm are:

* Public exponent: `3`
* Strength: `1024`
* Certainty: `25`