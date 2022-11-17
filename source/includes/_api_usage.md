# API usage

Sirius Validator API is a gRPC API. To get proto files please follow https://github.com/swisschain/Sirius.ValidatorApi.Docs/tree/master/.proto

To learn more about gRPC please follow https://grpc.io

## Pagination

Sirius API uses cursor-based pagination model in all endpoints which can return loads of items.

### Request

```csharp

var pagination = new PaginationInt64
{
    Cursor = 10008,
    Limit = 100,
    Order = PaginationOrder.Asc
}
                
```

name | type | description | example
---- | ---- | ----------- | -------
`cursor` | *optional*, *string* | Cursor to continue the search | stellar-test
`limit` | *optional*, *int* | Maximum number of items to return in the search results | 10
`order` | *optional*, *[Order](#api-usage-models-order-enum)* | Result items sorting order | asc



### Response

```csharp

var items = response.Body.Items;
var pagination = response.Body.Pagination;
                
```

name | type | description 
---- | ---- | ----------- 
`Items` | *List<T>* | The array of an objects
`Pagination` | *[Pagination](#api-usage-models-pagination-information)* | Pagination state to continue search

## Models

### Pagination information 

```csharp

var cursor = response.Body.Pagination.Cursor;
var count = response.Body.Pagination.Count;
var order = response.Body.Pagination.Order;
                
```

name | type | description | example
---- | ---- | ----------- | -------
`Cursor` | *optional*, *string* | Cursor to continue the search | stellar-test
`Count` | *optional*, *number* | Number of items in the current page | 10
`Order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc

### Order (enum)

Order of the sorting

+ `asc` - Ascending sorting order
+ `desc` - Descending sorting order
