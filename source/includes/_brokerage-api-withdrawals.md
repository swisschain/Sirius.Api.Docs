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

### Parameters

`document` field of the request should be a JSON-formatted document describing the withdrawals parameters.

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

> POST /api/withdrawals 200 OK

```json
{
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