# gRPC Protocol description

Sirius API uses gRPC protocol as the main one. To learn more about gRPC please follow [grpc.io](https://grpc.io)

There are several ways to use Sirius gRPC API. If you use .NET, the most convenient wayt for you is Sirius.Api.Client [![Nuget Package](https://img.shields.io/nuget/v/Swisschain.Sirius.Api.ApiClient.svg)](https://www.nuget.org/packages/Swisschain.Sirius.Api.ApiClient/).

Otherwise, you can get [proto files](https://github.com/swisschain/Sirius.Api.Docs/tree/master/.proto) directly and generate client on almost any platform.

## .NET client registration

```csharp

var options = new SiriusApiOptions
{
    ServerGrpcUrl = new Uri("https://sirius-grpc.swisschain.io"),
    ApiKey = "<you api key>",
    Timeout = TimeSpan.FromSeconds(60)
};

services.AddSiriusApiClient(options);

```

In `startup.cs` file in `ConfigureServices` method add API client using `IServiceCollection` extension method `AddSiriusApiClient`.

## Authentication

Sirius API uses `Bearer authentication`. Authorization token that a client needs to send with a request is a JWT token.
To get the token, [contact us](mailto:info@swisschain.io) to get invitation code, then register in our [Universe portal](https://universe.swisschain.io/), create a subscription and create an API key. We use JWT as the API key, so you can explore which claims particular token contains using [jwt.io](https://jwt.io) online service.

Authorization token should be passed as a gRPC metadata item with the key `Authorization` and value prefix `Bearer `:

name | type | placement | description | example
---- | ---- | --------- | ----------- | -------
`Authorization` | *string* | metadata | Sirius API key obtained via Universe portal prepended with the `Bearer ` prefix | `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJzaXJpdXMuc3dpc3NjaGFpbi5pbyIsImFwaS1rZXktaWQiOiJjM2U3NzhlNy0yM2JjLTQ3YzAtYmYxNC0wMWQ4ZGIxZjQ0YTciLCJ1bmlxdWVfbmFtZSI6IjIiLCJ0ZW5hbnQtaWQiOiJlNmY5Y2U3ZS1hZGFmLTRmNDgtYWI2ZC1lMjBiODk1YzRjZGEiLCIyZmEtZW5hYmxlZCI6IkZhbHNlIiwibmJmIjoxNjY5ODIyODI2LCJleHAiOjE2ODAyMTAwMDAsImlhdCI6MTY2OTgyMjgyNn0.04j2NB8e--mwhvXnEO5Hpd6khoh-Q5uuzT72xX06dFc`

For simplicity this metadata item is not shown in every request that requires authorization.

## Guid

Some API objects have string fields that in fact contain [Guid](http://guid.one/guid). Exact format is `xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx`

## JSON string

Although given API is a gRPC API, some gRPC objects may have string fields that contain some data serialized in the JSON format. Mainly such JSON strings are used
to represent data that should be signed. Data can be serialized to JSON using any serialization options, the only requirement - JSON should follow [RFC 8259](https://datatracker.ietf.org/doc/rfc8259)

## JSON string timestamp

Since timestamp representation is not fixed in the RFC 8259, given API uses textual representation following [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Exact format is `yyyy-MM-ddThh:mm:ss.ffffffZ`. Timestamps are always in UTC time zone

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

### Models

#### Pagination information 

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

#### Order (enum)

Order of the sorting

+ `asc` - Ascending sorting order
+ `desc` - Descending sorting order

