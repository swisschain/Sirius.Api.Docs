# Deposits

Deposits reflects incoming transfers of the funds either to an account or directly to the broker account. You can get deposits history, or be notified about new deposits or state changing of the existing deposits. There are different options to be notified about deposits state changing:

* You can pull deposit updates REST API
* You can listen for deposit updates gRPC stream
* You can register a web-hook get the notifications (coming soon)

Every deposit is associated with a broker account, an asset, optionally an account, it has amount, fees and state. If the account associated with the deposit has reference ID, you can find this value in the deposit too. 

In the case if the deposit amount is
too small to transfer funds from the account address to the broker account address because amount even doesn't cover fees or not significantly greater than fees, the deposited funds will be keept on the account address until the next deposit with enough amount. We use term "Tiny" for such deposits. If deposit destination is a broker account instead of an account, we name such deposits "Broker deposits".

The lifecycle of a deposit:

<img src="https://github.com/swisschain/Sirius.Api.Docs/raw/master/source/images/deposit-lyfecycle.png" alt="Deposit lifecycle"/>

## Searches deposits

### Request

**Rest API:** `GET /api/deposits`

**gRPC API:** `swisschain.sirius.api.deposits.Deposits.Search`

### Query Parameters

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`id` | `id` | *optional*, *number* | Exact deposit ID to search | 500000113
`brokerAccountId` | `broker_account_id` | *optional*, *number* | ID of the broker account  | 100000113
`state` | `state` | *optional*, *Array of [DepositState](#depositstate-enum)* | State of the deposit | detected
`type` | `deposit_type` | *optional*, *Array of [DepositType](#deposittype-enum)* | State of the deposit | regular
`accountId` | `accountId` | *optional*, *number* | ID of the account  | 200000113
`assetId` | `asset_id` | *optional*, *number* | ID of the asset  | 100000
`blockchainId` | `blockchain_id` | *optional*, *string* | ID of the blockchain  | ethereum-test
`referenceId` | `reference_id` | *optional*, *string* | Reference ID of the account  | ethereum-test
`order` | `pagination.order` | *optional*, *[Order](#order-enum)* | Result items sorting order | asc
`cursor` | `pagination.cursor` | *optional*, *string* | Cursor to continue the search | 11111110
`limit` | `pagination.limit` | *optional*, *number* | Maximum number of items to return in the search results | 10


```protobuf
swisschain.sirius.api.deposits.Deposits.Search

> Requets: (application/grpc)

message DepositSearchRequest {
  google.protobuf.Int64Value id = 1;
  google.protobuf.Int64Value broker_account_id = 2;
  google.protobuf.Int64Value account_id = 3;
  google.protobuf.StringValue reference_id = 4;
  google.protobuf.Int64Value asset_id = 5;
  google.protobuf.StringValue blockchain_id = 6;
  repeated DepositState state = 7;
  swisschain.sirius.api.common.PaginationInt64 pagination = 8;
  repeated DepositType deposit_type = 9;
}
```

### Response

```json
GET /api/deposits

> Response: 200 (application/json) - success response

{
    "pagination":{
        "cursor":null,
        "count":10,
        "order":"desc",
        "nextUrl":"/api/deposits?Order=Desc&Cursor=105000380&Limit=10"
        },
        "items":[
            {
         "id":105000389,
         "brokerAccountId":100000113,
         "brokerAccountDetails":{
            "id":101000254,
            "address":"0x02d45A855dB2dD98899cdA9a58c16E8f4A1faA96"
         },
         "accountId":null,
         "referenceId":null,
         "accountDetails":null,
         "assetId":100004,
         "blockchainId":"ethereum-ropsten",
         "fees":[
            
         ],
         "transactionInfo":{
            "transactionId":"0x29f5b19e76977118d96dba56306c46b6a5406d12da11a1f20add96d002b6d139",
            "transactionBlock":9890232,
            "confirmationsCount":58738,
            "requiredConfirmationsCount":3,
            "date":"2021-03-22T15:40:58Z"
         },
         "state":"completed",
         "sources":[
            {
               "address":"0x1Eab7d412a25a5d00Ec3d04648aa54CeA4aB7e94",
               "amount":0.3803696
            }
         ],
         "amount":0.38,
         "createdAt":"2021-03-22T15:41:12.43838Z",
         "updatedAt":"2021-03-22T15:42:22.412655Z",
         "requiredConfirmationsCount":3,
         "assetSymbol":"ETH",
         "assetAddress":null,
         "blockchainName":"Ethereum test network",
         "brokerAccountName":"AmlCheck-3",
         "depositType":"broker",
         "consolidationTransactionInfo":null,
         "provisioningTransactionInfo":null,
         "amlChecks":{
            "overallResolution":"succeeded",
            "startedAt":"2021-03-22T19:42:15.178434+04:00",
            "completedChecks":[
               {
                  "amlCheckId":601000004,
                  "amlConnectionId":600000012,
                  "resolution":"unknown",
                  "finishedAt":"2021-03-22T15:42:19.2085083+00:00"
               }
            ],
            "requiredChecks":1
         },
         "sequence":3
      }, ...]
}
                

```protobuf
swisschain.sirius.api.deposits.Deposits.Search

> Response: (application/grpc) - success response

message DepositSearchResponse {
  oneof result {
    DepositSearchResponseBody body = 1;
    swisschain.sirius.api.common.ErrorResponseBody error = 2;
  } 
}

message DepositSearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;
  repeated DepositResponse items = 2;
}

message DepositResponse {
  int64 id = 1;
  int64 broker_account_id = 2;
  swisschain.sirius.api.common.BrokerAccountDetails broker_account_details = 3;
  google.protobuf.Int64Value account_id = 4;
  google.protobuf.StringValue reference_id = 5;
  swisschain.sirius.api.common.AccountDetails account_details = 6;
  int64 asset_id = 7;
  string blockchain_id = 8;
  repeated swisschain.sirius.api.common.Fee fees = 9;
  swisschain.sirius.api.common.TransactionInfo transaction_info = 10;
  DepositState state = 11;
  repeated DepositSource sources = 12;
  swisschain.sirius.api.common.BigDecimal amount =13;
  google.protobuf.Timestamp created_at = 14;
  google.protobuf.Timestamp updated_at = 15;   
  int64 required_confirmations_count = 16;
  string asset_symbol = 17;
  google.protobuf.StringValue asset_address = 18;
  string blockchain_name = 19;
  string broker_account_name = 20;
  DepositType deposit_type = 21;
  swisschain.sirius.api.aml.AmlChecks aml_checks = 22;
}
```

Paginated response of the deposits:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------


## Searches deposit updates

### Request

**Rest API:** `GET /api/deposit-updates`

### Parameters

### Query Parameters

### Response

> GET /api/deposit-updates 200 OK

```json
{
}
```