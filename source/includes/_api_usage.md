# API usage

Sirius Validator API is a gRPC API. To get proto files please follow [our GitHub](https://github.com/swisschain/Sirius.ValidatorApi.Docs/tree/master/.proto)

To learn more about gRPC please follow [grpc.io](https://grpc.io)

## Authentication

Sirius Validator API uses `Bearer authentication`. Authentication token that a client needs to send with a request is a JWT token.
The token can be obtained during the Validator registration process that can be initiated via [Universe portal](https://universe.swisschain.io).
See *[Invites](#invites)* section to get more details.

> Request Header

```
  "Authorization": "Bearer **********************************"
```

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