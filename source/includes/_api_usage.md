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

## Pagination

Some API endpoint that potentially returns a lot of items have pagination option. Request object of such an endpoint has *[Page](#api-usage-pagination-page-object)* field with name `page`.


### Page object

```proto
message Page {
  int32 index = 1;
  int32 size = 2;
}

message ARequest {
  Page page = 1;
}
```

name | type | description | contraints | example
---- | ---- | ----------- | ---------- | -------
`index` | *in32* | Zero-based index of the page to query | index >= 0 | 10
`size` | *int32* | Maximum number of items to return in the results | size >= 0 and size <= 100 | 50