# Accounts

Account is the multi-currency (multi-blockchain) account which is associated with a particular broker account. Account has the same blockchains enabled as the parent broker account has. Account can be associated with a thing in your system by using the "reference ID". You can pass ID of your end-user, invoice, contract or any other things with which you want to associate deposits received on the account. Reference ID is optional, so you can avoid passing your internal IDs to Sirius and keep mapping between your objects and Sirius accounts on your side.

## Create an account

### Request

**Rest API:** `POST /api/accounts`

**gRPC API:** `swisschain.sirius.api.accounts/Create`

```json
POST /api/accounts

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
    "brokerAccountId": 100000113
    "referenceId": "user reference"
    "userId": 108000004
}
```

```protobuf
swisschain.sirius.api.accounts/Create

> Requets: (application/grpc)

message AccountCreateRequest {
  string request_id = 1;                        //1a5c0b3d15494ec8a390fd3b22d757d6

  int64 broker_account_id = 2;                  //100000113
  
  google.protobuf.StringValue reference_id = 3; //user reference (could be empty)

  google.protobuf.Int64Value user_id = 4;       //108000004 (could be empty)
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`brokerAccountId` | `broker_account_id` | *optional*, *number* | *body* | Id of the related Broker Account
`referenceId` | `reference_id` | *optional*, *string* | *body* | Account reference id
`userId` | `user_id` | *optional*, *number* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)

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

```protobuf
swisschain.sirius.api.accounts/Create

> Response: (application/grpc) - success response

message AccountCreateResponse {
    oneof result {
      AccountCreateResponseBody body = 1;                         // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2;   // NULL
    } 
}

message AccountCreateResponseBody {
  AccountResponse account = 1;                                      // Object
}

message AccountResponse {
  int64 id = 1;                                                     // 103000158
  string reference_id = 2;                                          // "user reference" 
  int64 broker_account_id = 3;                                      // 100000113
  AccountStateModel state = 4;                                      // AccountStateModel.Creating
  google.protobuf.Timestamp createdAt = 5;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Timestamp updatedAt = 6;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Int64Value user_id = 7;                           // 108000004
  google.protobuf.StringValue user_native_id = 8;                   // "chainalysis-user-1"
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the account
`referenceId` | `reference_id` | *optional*, *string* | *body* | Account reference id
`brokerAccountId` | `broker_account_id` | *number* | *body* | Id of the related Broker Account
`status` | `status` | *[AccountState](#account-state)* | Status of the account
`createdAt` | `created_at` | *timestamp* | Date of the account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest account update
`userId` | `user_id` | *optional*, *number* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)
`userNativeId` | `user_native_id` | *optional*, *string* | *body* | Native Id of the user in customer's system


## Search for accounts

### Request

**Rest API:** `GET /api/accounts`

**gRPC API:** `swisschain.sirius.api.accounts/Search`

### Query Parameters

REST name | gRPC name | type | REST placement | description | example
---- | ---- | ----------- | -------
`id` | `id` | *optional*, *number* | ID of the account to search | 11111111
`brokerAccountId` | `broker_account_id` | *optional*, *number* | Exact broker account id to search | 100000113
`userId` | `user_id` | *optional*, *number* | ID of users | 108000004
`referenceId` | `reference_id` | *optional*, *string* | Account reference id | user reference
`state` | `state` | *optional*, *[AccountState](#account-state)* | State of the account | creating
`order` | `pagination.order` | *optional*, *[Order](#order)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


```protobuf
swisschain.sirius.api.accounts/Search

> Requets: (application/grpc)

message AccountSearchRequest {
  google.protobuf.Int64Value id = 1;                            // 11111111
  google.protobuf.Int64Value broker_account_id = 2;             // 100000113
  repeated AccountStateModel state = 3;                         // [AccountStateModel.Creating, AccountStateModel.Active, AccountStateModel.Blocked]
  google.protobuf.StringValue reference_id = 4;                 // user reference
  swisschain.sirius.api.common.PaginationInt64 pagination = 5;  // Pagination object
  google.protobuf.Int64Value user_id = 6;                       //108000004 (could be empty)
}
```

### Response

```json
POST /api/accounts 

> Response: 200 (application/json) - success response

{
    "pagination":
    {
        "cursor":103000021,
        "count":10,
        "order":"asc",
        "nextUrl":"/api/accounts/details?Order=Asc&Cursor=103000031&Limit=10"
    },
    "items":[
        {
            "id":103000022,
            "referenceId":"some ref7",
            "brokerAccountId":100000019,
            "state":"active",
            "createdAt":"2020-09-14T16:25:54.831938+00:00",
            "updatedAt":"2020-09-14T16:28:14.536684+00:00",
            "userId":null,
            "userNativeId":null},]
}
```

```protobuf
swisschain.sirius.api.accounts/Search

> Response: (application/grpc) - success response

message AccountSearchResponse {
    oneof result {
      AccountSearchResponseBody body = 1;                         // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2;   // NULL
    } 
}

message AccountSearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Pagination

  repeated AccountResponse items = 2;                                   // Array of objects
}

message AccountResponse {
  int64 id = 1;                                                     // 103000158
  string reference_id = 2;                                          // "user reference" 
  int64 broker_account_id = 3;                                      // 100000113
  AccountStateModel state = 4;                                      // AccountStateModel.Creating
  google.protobuf.Timestamp createdAt = 5;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Timestamp updatedAt = 6;                          // "2021-03-26T14:14:46.089822+00:00"
  google.protobuf.Int64Value user_id = 7;                           // 108000004
  google.protobuf.StringValue user_native_id = 8;                   // "chainalysis-user-1"
}
```

## Searches the account details

### Request

**Rest API:** `GET /api/accounts/{accountId}/details`

### Parameters

### Query Parameters

### Response

> GET /api/accounts/{accountId}/details 200 OK

```json
{
}
```

## Gets the account details by the asset id

### Request

**Rest API:** `GET /api/accounts/{accountId}/details/by-asset-id/{assetId}`

### Parameters

### Query Parameters

### Response

> GET /api/accounts/{accountId}/details/by-asset-id/{assetId} 200 OK

```json
{
}
```