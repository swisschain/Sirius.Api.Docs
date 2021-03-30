# Broker accounts

Broker account is the multi-currency (multi-blockchain) account which can contain unlimited number of personalized accounts within it. Broker account aggregates funds deposited to the personalized accounts and it provides funds for the withdrawals. The private keys of the broker accounts are stored in the vault associated with the broker account. The vault can't be changed once broker account is created.

You can choose which particular blockchain should be enabled for the given broker account. You can give a name to the broker account which make sense for you. It's convenient to have separate broker account for each business case. It's also reasonable to keep funds on the different brokerage accounts because of security reasons. For instance you can have a "Hot" broker account for operational work, "Cold" broker account to keep reserve funds which can be used by the limited employees group of your company, and "Frozen" broker account which can be used only by the owners of the business. Then you can setup a process to settle funds between these broker accounts on a periodical basis or on a demand.

Under the hood a broker account manages set of blockchain addresses. It has at least one address for each blockchain enabled for it. Depending on a particularities of each blockchain separate address or unique destination tag is created for each account within the broker account. Deposits received on an address of an account automatically moved to the address of the broker account. For some assets it's needed to pay fees in a different asset to move funds from an account to the broker account. For this Sirius provides "fees assets" on the account address by moving them from the broker account address.

Broker account structure:

<img src="https://github.com/swisschain/Sirius.Api.Docs/raw/master/source/images/broker-account-structure.png" alt="Broker account structure lifecycle"/>

## Create a broker account

### Request

**Rest API:** `POST /api/broker-accounts`

**gRPC API:** `swisschain.sirius.api.brokerAccounts/Create`

```json
POST /api/broker-accounts

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
    "amlConnectionIds": [ 600000012 ],
    "blockchainIds": [ "bitcoin-test", "ethereum-ropsten"],
    "custodyId": 404000009,
    "name": "Broker Account Name"
}
```

```protobuf
swisschain.sirius.api.brokerAccounts/Create

> Requets: (application/grpc)

message BrokerAccountCreateRequest {
  string request_id = 1;                    // 1a5c0b3d15494ec8a390fd3b22d757d6
  string name = 2;                          // "Broker Account Name"
  int64 custody_id = 3;                     // 404000009
  repeated string blockchain_ids = 4;       // [ "bitcoin-test", "ethereum-ropsten"] Array of blockchain ids
  repeated int64 aml_connection_ids = 5;    // [ 600000012 ] Array of aml connections ids
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`name` | `name` | *string* | *body* | Name of the created Broker Account
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`blockchainIds` | `blockchain_ids` | *Array of strings* | *body* | Ids of the blockchains with which broker Account should work
`amlConnectionIds` | `aml_connection_ids` | *Array of strings* | *body* | Ids of the AML connections to enable for deposit/withdrawal checks

### Response

```json
POST /api/broker-accounts 

> Response: 200 (application/json) - success response

{
    "name":"Broker Account Name",
    "id":100000115,
    "state":"creating",
    "accountCount":0,
    "blockchainsCount":2,"
    createdAt":"2021-03-30T09:02:44.7726151+00:00",
    "updatedAt":"2021-03-30T09:02:44.7726151+00:00",
    "custodyId":404000009,
    "custodyName":"NewSharedHSM",
    "blockchainIds":["bitcoin-test","ethereum-ropsten"],
    "amlConnectionIds":[600000012]
}
```

```protobuf
swisschain.sirius.api.brokerAccounts/Create

> Response: (application/grpc) - success response

message BrokerAccountCreateResponse {
    oneof result {
      BrokerAccountCreateResponseBody body = 1;
      swisschain.sirius.api.common.ErrorResponseBody error = 2;
    } 
}

message BrokerAccountCreateResponseBody {
  BrokerAccountResponse broker_account = 1;
}

message BrokerAccountResponse {
  string name = 1;
  int64 id = 2;
  BrokerAccountState state = 3;
  int64 accounts_count = 4;
  int64 blockchains_count = 5;
  google.protobuf.Timestamp created_at = 6;
  google.protobuf.Timestamp updated_at = 7;
  int64 custody_id = 8;
  string custody_name = 9;
  repeated string blockchain_ids = 10;
  repeated int64 aml_connection_ids = 11;
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account
`name` | `name` | *string* | *body* | Account reference id
`accountsCount` | `accounts_count` | *number* | Number of accounts that are attached to the broker account
`blockchainsCount` | `blockchains_count` | *number* | Number of already created broker account details
`state` | `state` | *[BrokerAccountState](#broker-account-state)* | Status of the broker account
`createdAt` | `created_at` | *timestamp* | Date of the broker account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest broker account update
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchain_ids` | `blockchain_ids` | *Array of string* | *body* | Blockchains ids that are connected to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of numbers* | *body* | AML Connections that are enabled for the broker account


## Update the broker account

### Request

**Rest API:** `PUT /api/broker-accounts`

**gRPC API:** `swisschain.sirius.api.brokerAccounts/Update`

```json
PUT /api/broker-accounts

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
    "amlConnectionIds": [600000007, 600000012],
    "blockchainIds": ["stellar-test"],
    "brokerAccountId": 100000116
}
```

```protobuf
swisschain.sirius.api.brokerAccounts/Update

> Requets: (application/grpc)

message BrokerAccountUpdateRequest {
  string request_id = 1;                    // 1a5c0b3d15494ec8a390fd3b22d757d6
  int64 broker_account_id = 2;              // 100000116 
  repeated string blockchain_ids = 3;       // ["stellar-test"] only new ids could be added
  repeated int64 aml_connections_ids = 4;   // [600000007, 600000012] already existing ids could be removed
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`brokerAccountId` | `broker_account_id` | *number* | *body* | ID of the broker account
`blockchainIds` | `blockchain_ids` | *Array of strings* | *body* | Ids of the blockchains to add to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of strings* | *body* | Ids of the AML connections to enable for deposit/withdrawal checks

### Response

```json
PUT /api/broker-accounts 

> Response: 200 (application/json) - success response

{
    "name":"Broker Account Docs",
    "id":100000116,
    "state": "updating",
    "accountCount":0,
    "blockchainsCount":3,
    "createdAt":"2021-03-30T09:49:47.298466+00:00",
    "updatedAt":"2021-03-30T09:51:54.3471023+00:00",
    "custodyId":404000008,
    "custodyName": "SharedHSM_MAIN",
    "blockchainIds": ["bitcoin-test","ethereum-ropsten","stellar-test"],
    "amlConnectionIds":[600000007,600000012]
}
```

```protobuf
swisschain.sirius.api.brokerAccounts/Update

> Response: (application/grpc) - success response

message BrokerAccountUpdateResponse {
  oneof result {
    BrokerAccountUpdateActionResponseBody body = 1;
    swisschain.sirius.api.common.ErrorResponseBody error = 2;
  } 
}

message BrokerAccountUpdateActionResponseBody {
  BrokerAccountResponse broker_account = 1;
}

message BrokerAccountResponse {
  string name = 1;
  int64 id = 2;
  BrokerAccountState state = 3;
  int64 accounts_count = 4;
  int64 blockchains_count = 5;
  google.protobuf.Timestamp created_at = 6;
  google.protobuf.Timestamp updated_at = 7;
  int64 custody_id = 8;
  string custody_name = 9;
  repeated string blockchain_ids = 10;
  repeated int64 aml_connection_ids = 11;
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the broker account
`name` | `name` | *string* | *body* | Account reference id
`accountsCount` | `accounts_count` | *number* | Number of accounts that are attached to the broker account
`blockchainsCount` | `blockchains_count` | *number* | Number of already created broker account details
`state` | `state` | *[BrokerAccountState](#broker-account-state)* | Status of the broker account
`createdAt` | `created_at` | *timestamp* | Date of the broker account creation
`updatedAt` | `updated_at` | *timestamp* | Date of the latest broker account update
`custodyId` | `custody_id` | *number* | *body* | ID of the custody for broker account
`custodyName` | `custody_name` | *string* | *body* | Name of the custody related to broker account
`blockchain_ids` | `blockchain_ids` | *Array of string* | *body* | Blockchains ids that are connected to the broker account
`amlConnectionIds` | `aml_connection_ids` | *Array of numbers* | *body* | AML Connections that are enabled for the broker account

## Searches broker accounts

### Request

**Rest API:** `GET /api/broker-accounts`

### Parameters

### Query Parameters

### Response

> GET /api/broker-accounts 200 OK

```json
{
}
```

## Searches the broker account balances

### Request

**Rest API:** `GET /api/broker-accounts/{brokerAccountId}/balances`

### Parameters

### Query Parameters

### Response

> GET /api/broker-accounts/{brokerAccountId}/balances 200 OK

```json
{
}
```

## Searches the broker account details

### Request

**Rest API:** `GET /api/broker-accounts/{brokerAccountId}/details`

### Parameters

### Query Parameters

### Response

> GET /api/broker-accounts/{brokerAccountId}/details 200 OK

```json
{
}
```

## Gets the broker account details by the asset id

### Request

**Rest API:** `GET /api/broker-accounts/{brokerAccountId}/details/by-asset-id/{assetId}`

### Parameters

### Query Parameters

### Response

> GET /api/broker-accounts/{brokerAccountId}/details/by-asset-id/{assetId} 200 OK

```json
{
}
```