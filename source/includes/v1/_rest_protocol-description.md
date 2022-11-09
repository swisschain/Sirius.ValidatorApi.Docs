# REST Protocol description

## Allowed HTTP Verbs

- `PUT` : Updates a resource.
- `POST` : Creates a resource.
- `GET` : Gets a resource or a list of resources.
- `DELETE` : Deletes a resource.

## Description Of HTTP Server Responses

- 200 `OK` : the request was successful.
- 201 `Created` : the request was successful and a resource was created.
- 204 `No Content` : the request was successful but there is no representation to return.
- 400 `Bad Request` : the request has invalid or missing required parameters.
- 401 `Unauthorized` : authentication failed.
- 403 `Forbidden` : access denied.
- 404 `Not Found` : resource was not found.

## Authentication

Sirius API uses `Bearer authentication`. To get the token, [contact us](mailto:info@swisschain.io) to get invitation code, then register in our [Universe portal](https://universe.swisschain.io/), create a subscription and create an API key. We use JWT as the API key, so you can explore which claims particular token contains using [jwt.io](https://jwt.io) online service.

> Request Header

```json
  "Authorization": "Bearer **********************************"
```

## Error responses

Until other is specified for the particular endpoint and HTTP status code, the error response model is:

- `errors` - errors map, where key is a request field name.
  - `""` (array[string]) - (the key is empty string) the array of the summary errors which are not associated with any request fields directly.
  - `propertyName` (array[string]) - the array of the errors associated with the propertyName field of the request.


> Error response.

```json
{
  "errors": {
    "": [
      "Errors summary"
    ],
    "order": [
      "Invalid order"
    ],
    "limit": [
      "Limit should be in the range 1..1000"
    ]
  }
}
```

## Pagination

Sirius API uses cursor-based pagination model in all endpoints which can return loads of items.

Pagination parameters are passed via query string. The parameters can be:

- `order` : (***optional***, **Order**) - sorting order of the result. Items always sorted by the ID.
- `cursor` : (***optional***) ID of the item, after which result should be returned. Type of the cursor is the same as the ID type of the queried items.
- `limit` : (***optional***, **number**) specifies how many items should be returned at maximum.

The response is:

### Paginated

Paginated response

- `pagination` - pagination state.
  - `cursor` - current cursor.
  - `count` (number) - count of the items in the response.
  - `order` (Order) - current order.
  - `nextUrl` (string) - relative URL of the next items page.
  - `items` (array[object]) - the array of the queried items.

## Decimal number

Here you can see how `decimal` numbers are presented (Price, Volume, Amount, etc) in API contract.

In the `Rest API`, the decimal type is presented as a number with strict precision.

> Decimal number example

```json
{
    "price": 222231.33420001911,
    "volume": 0.0000001
}
```

## Timestamp

`iso 8601` is used for the timestamps in the form `2020-08-04T08:05:09.039924+00:00`

> Timestamp example 

```json
{
   "Timestamp": "2020-08-04T08:05:09.039924+00:00"
}
```
