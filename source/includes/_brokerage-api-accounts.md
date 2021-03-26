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
`brokerAccountId` | `broker_account_id` | *int64* | *body* | Id of the related Broker Account
`referenceId` | `reference_id` | *StringValue* | *body* | Account reference id
`userId` | `user_id` | *Int64Value* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)

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
`referenceId` | `reference_id` | *StringValue* | *body* | Account reference id
`brokerAccountId` | `broker_account_id` | *int64* | *body* | Id of the related Broker Account
`status` | `status` | *[AccountState](#account-state)* | Status of the account
`createdAt` | `created_at` | *timestamp* | Date of the account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest account update
`userId` | `user_id` | *Int64Value* | *body* | Id of the related user in the system (Needed for enabling AML on broker Account)
`userNativeId` | `user_native_id` | *StringValue* | *body* | Native Id of the user in customer's system

## Searches accounts

### Request

**Rest API:** `GET /api/accounts`

### Parameters

### Query Parameters

### Response

> GET /api/accounts 200 OK

```json
{
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