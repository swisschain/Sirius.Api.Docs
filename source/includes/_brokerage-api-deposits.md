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
swisschain.sirius.api.brokerAccounts/Search

> Response: (application/grpc) - success response

message BrokerAccountSearchResponse {
  oneof result {
    BrokerAccountSearchResponseBody body = 1;                   // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;   // Object
  } 
}

message BrokerAccountSearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated BrokerAccountResponse items = 2;                             // Object
}

message BrokerAccountResponse {
  string name = 1;                                              // "Broker Account Name"
  int64 id = 2;                                                 // 100000115
  BrokerAccountState state = 3;                                 // BrokerAccountState.Creating
  int64 accounts_count = 4;                                     // 0
  int64 blockchains_count = 5;                                  // 2
  google.protobuf.Timestamp created_at = 6;                     // "2021-03-30T09:02:44.7726151+00:00"
  google.protobuf.Timestamp updated_at = 7;                     // "2021-03-30T09:02:44.7726151+00:00"
  int64 custody_id = 8;                                         // 404000009
  string custody_name = 9;                                      // "NewSharedHSM"
  repeated string blockchain_ids = 10;                          // ["bitcoin-test","ethereum-ropsten"]
  repeated int64 aml_connection_ids = 11;                       // [600000012]
}
```

Paginated response of the broker accounts:

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account
`name` | `name` | *string* | *body* | Broker account name
`accountsCount` | `accounts_count` | *number* | Number of accounts that are attached to the broker account
`blockchainsCount` | `blockchains_count` | *number* | Number of already created broker account details
`state` | `state` | *[BrokerAccountState](#brokeraccountstate-enum)* | Status of the broker account
`createdAt` | `created_at` | *timestamp* | Date of the broker account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest broker account update
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchain_ids` | `blockchain_ids` | *Array of string* | *body* | Blockchains ids that are connected to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of numbers* | *body* | AML Connections that are enabled for the broker account


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