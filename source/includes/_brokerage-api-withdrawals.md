# Withdrawals

Withdrawals reflects outgoing transfers of the funds from a broker account. You can get withdrawals history, or be notified about new withdrawals and state changing of the existing withdrawals. There are different options to be notified about withdrawals state changing:

* You can pull withdrawals updates REST API
* You can listen for withdrawal updates gRPC stream
* You can register a web-hook get the notifications (coming soon)

Withdrawal can be initiated by you via API or Universe portal UI. Every withdrawal is associated with a broker account, an asset, optionally an account, it has amount, fees and state. If you use reference ID for the accounts, you can specify either account ID or reference ID, to associate an account to the withdrawal. This link doesn't affect funds flow, it's used just for the reporting.

The fees required for the withdrawal transaction are paid atop of the specified amount.

The lifecycle of a withdrawal:

<img src="https://github.com/swisschain/Sirius.Api.Docs/raw/master/source/images/withdrawal-lyfecycle.png" alt="Withdrawal lifecycle"/>

## Starts a withdrawal

### Request

**Rest API:** `POST /api/withdrawals`

```json
POST /api/withdrawals

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "document": "{\"version\":\"1.0\",\"brokerAccountId\":100000162,\"accountId\":103000171,\"accountReferenceId\":99,\"withdrawalReferenceId\":76766,\"assetId\":112248,\"amount\":\"0.053\",\"destinationDetails\":{\"address\":\"0x45238387503b485ae1a279d8B420f0e8562bfa35\",\"tag\":null,\"tag_type\":null}}",
  "signature": "CBAirYR2d6vmRV6d1GEfdA3Kv11pOvdebhEyx8pGNKvTaQcbc0IiBUKCESX9eiOlxorkMDuOEMVs4HZ6JnKg6aY8akb72723cBTxvNIHb/P3OyHQWECcIKwmruWXEunWS8ZlFjxy/nWr/XGD0G8N8/xREGhGCcHCjdwrMmMQBSg="
}


```

```protobuf
swisschain.sirius.api.withdrawals.Withdrawals.Execute

> Requets: (application/grpc)

message WithdrawalExecuteRequest {
    string request_id = 1; // 3a2b59a1-07e5-48b1-ba8b-4b3c750a2aa1
    string document = 2; // "{\"version\":\"1.0\",\"brokerAccountId\":100000162,\"accountId\":103000171,\"accountReferenceId\":99,\"withdrawalReferenceId\":76766,\"assetId\":112248,\"amount\":\"0.053\",\"destinationDetails\":{\"address\":\"0x45238387503b485ae1a279d8B420f0e8562bfa35\",\"tag\":null,\"tag_type\":null}}"
    string signature = 3; // "CBAirYR2d6vmRV6d1GEfdA3Kv11pOvdebhEyx8pGNKvTaQcbc0IiBUKCESX9eiOlxorkMDuOEMVs4HZ6JnKg6aY8akb72723cBTxvNIHb/P3OyHQWECcIKwmruWXEunWS8ZlFjxy/nWr/XGD0G8N8/xREGhGCcHCjdwrMmMQBSg="
}

```

### Parameters

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`document` | `document` | *string* | *body* | JSON-formatted document describing the withdrawals parameters
`signature` | `signature` | *string* | *body* | document signature

```
Withdral document format:

{
    "version": "string", // "1.0"
    "brokerAccountId": 1000000,
    "accountId": 1000000, // Optional
    "accountReferenceId": "string", // Optional
    "withdrawalReferenceId": "string", // Optional
    "assetId": 1000000,
    "amount": 12.345,
    "destinationDetails": {
        "address": "string", // Blockchain address
        "tag": "string", // Optional
        "tagType": "string", // Optional - ("text", "number")
    }
}
```

### Query Parameters

### Response


```json
POST /api/withdrawals

> Response: 200 (application/json) - success response

{
  "id": 0,
  "brokerAccountId": 0,
  "brokerAccountName": "string",
  "brokerAccountDetails": {
    "id": 0,
    "address": "string"
  },
  "accountId": 0,
  "accountDetails": {
    "id": 0,
    "address": "string",
    "tag": "string",
    "tagType": "text"
  },
  "assetId": 0,
  "assetSymbol": "string",
  "assetAddress": "string",
  "amount": 0,
  "blockchainId": "string",
  "blockchainName": "string",
  "fees": [
    {
      "assetId": 0,
      "amount": 0
    }
  ],
  "destinationDetails": {
    "address": "string",
    "tag": "string",
    "tagType": "text"
  },
  "state": "processing",
  "transactionInfo": {
    "transactionId": "string",
    "transactionBlock": 0,
    "confirmationsCount": 0,
    "requiredConfirmationsCount": 0,
    "date": "2021-03-31T08:09:46.258Z"
  },
  "error": {
    "message": "string",
    "code": "notEnoughBalance"
  },
  "operationId": 0,
  "requiredConfirmationsCount": 0,
  "transferContext": {
    "accountReferenceId": "string",
    "withdrawalReferenceId": "string",
    "document": "string",
    "signature": "string",
    "requestContext": {
      "userId": "string",
      "apiKeyId": "string",
      "ip": "string",
      "timestamp": "2021-03-31T08:09:46.258Z"
    }
  },
  "validatorContext": {
    "document": "string",
    "signature": "string"
  },
  "createdAt": "2021-03-31T08:09:46.258Z",
  "updatedAt": "2021-03-31T08:09:46.258Z"
}
```

```protobuf
swisschain.sirius.api.withdrawals.Withdrawals.Execute

> Response: (application/grpc) - success response

message WithdrawalExecuteResponse {
    oneof result {
        WithdrawalExecuteResponseBody body = 1;
        WithdrawalExecuteErrorResponseBody error = 2;
    }
}

message WithdrawalExecuteResponseBody {
    WithdrawalResponse withdrawal = 1;
}

message WithdrawalResponse {
    int64 id = 1;
    int64 broker_account_id = 2;
    string broker_account_name = 3;
    swisschain.sirius.api.common.BrokerAccountDetails broker_account_details = 4;
    google.protobuf.Int64Value account_id = 5;
    swisschain.sirius.api.common.AccountDetails account_details = 6;
    google.protobuf.StringValue account_reference_id = 7;
    int64 asset_id = 8;
    string asset_symbol = 9;
    google.protobuf.StringValue asset_address = 10;
    swisschain.sirius.api.common.BigDecimal amount = 11;
    string blockchain_id = 12;
    string blockchain_name = 13;
    repeated swisschain.sirius.api.common.Fee fee = 14;
    DestinationDetails destination_details = 15;
    WithdrawalState state = 16;
    swisschain.sirius.api.common.TransactionInfo transaction_info = 17;
    WithdrawalError error = 18;
    google.protobuf.Int64Value operation_id = 19;
    int64 required_confirmations_count = 20;
    TransferContext transfer_context = 21;
    ValidatorContext validator_context = 22;
    google.protobuf.Timestamp created_at = 23;
    google.protobuf.Timestamp updated_at = 24;
}
```

## Searches withdrawals

### Request

**Rest API:** `GET /api/withdrawals`

### Parameters

### Query Parameters

### Response

> GET /api/withdrawals 200 OK

```json
{
}
```

## Searches withdrawal updates

### Request

**Rest API:** `GET /api/withdrawal-updates`

### Parameters

### Query Parameters

### Response

> GET /api/withdrawal-updates 200 OK

```json
{
}
```