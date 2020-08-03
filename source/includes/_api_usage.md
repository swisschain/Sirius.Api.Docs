
# Protocol description


# API usage

## Allowed HTTP Verbs

- `PUT` : Updates a resource.
- `POST` : Creates a resource.
- `GET` : Gets a resource or a list of resources.
- `DELETE` : Deletes a resource.

## Description Of Usual HTTP Server Responses

- 200 `OK` : the request was successful.
- 201 `Created` : the request was successful and a resource was created.
- 204 `No Content` : the request was successful but there is no representation to return.
- 400 `Bad Request` : the request has invalid or missing required parameters.
- 401 `Unauthorized` : authentication failed.
- 403 `Forbidden` : access denied.
- 404 `Not Found` : resource was not found.

## Authentication

Sirius API uses `Bearer authentication`. To get the token, register in our Universe portal, create a subscription and create an API key. We use JWT as the API key, so you can explore which claims particular token contains using jwt.io online service.

> Request Header

```json
  "Authorization": "Bearer **********************************"
```

```protobuf
  "Authorization": "Bearer **********************************"
```

## Error responses

Until other is specified for the particular endpoint and HTTP status code, the error response model is:

- `errors` - errors map, where key is a request field name.
  - `""` (array[string]) - (the key is empty string) the array of the summary errors which are not associated with any request fields directly.
  - `propertyName` (array[string]) - the array of the errors associated with the propertyName field of the request.


> Successful response. Property `error` is null.

```json
{
    "some_data": {
        ...
    },
    "some_message": "Hello world",
    "error": null
}
```

```protobuf
package hft;

message Response {
    common.Error error = 1;  // error is NULL
    SomeDataType some_data = 2;
    string some_message = 3;
}
```

> Error response. Property `error` is not null.

```json
{
  "errors": {
    "order": [
      "Invalid order"
    ],
    "limit": [
      "Limit should be in the range 1..1000"
    ]
  }
}```

```protobuf

# todo: fill example

```

## Pagination

Sirius API uses cursor-based pagination model in all endpoints which can return loads of items.

Pagination parameters are passed via query string. The parameters can be:

- `order` : (***optional***, **Order**) - sorting order of the result. Items always sorted by the ID.
- `cursor` : (***optional***) ID of the item, after which result should be returned. Type of the cursor is the same as the ID type of the queried items.
- `limit` : (***optional***, **number**) specifies how many items should be returned at maximum.

The response is:

### Paginated

- `pagination` - pagination state.
  - `cursor` - current cursor.
  - `count` (number) - count of the items in the response.
  - `order` (Order) - current order.
  - `nextUrl` (string) - relative URL of the next items page.
  - `items` (array[object]) - the array of the queried items.

## Decimal type

Here you can see how to manage `decimal` types (Price, Volume, Amount, etc) in API contract.

In the `gRPC API` contract, the `decimal` type is presented as a `string` type, with a textual representation of the number. This is done in order to avoid issues with the non-strict precision `double` type.

In the `Rest API` contact, the decimal type is presented as a number with strict precision.

> Example with decimal type

```json
{
    "price": 222231.33420001911,
    "volume": 0.0000001
}
```

```protobuf
message Body {
    string price = 1;   // "222231.33420001911"
    string volume = 2;  // "0.0000001"
}
```

## Timestamp type
Here you can see: How to manage the `TimeStamp` type in the API contract.

<i>The timestamp is always used in the <b>time zone UTC+0</b></i>

In the `Rest API` contact, the `TimeStamp` type is presented as a `number` with "Milliseconds in Unix Epoch" format of date-time.

In the `gRPC API` contract, the `TimeStamp` type is presented as a `google.protobuf.TimeStamp` type.

> Example with timestamp

```json
{
   "Timestamp": 1592903724406
}
```

```protobuf
import "google/protobuf/timestamp.proto";

google.protobuf.Timestamp time_name = 1;  // 1592903724406
```
