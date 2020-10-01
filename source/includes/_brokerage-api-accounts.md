# Accounts

Account is the multi-currency (multi-blockchain) account which is associated with a particular broker account. Account has the same blockchains enabled as the parent broker account has. Account can be associated with a thing in your system by using the "reference ID". You can pass ID of your end-user, invoice, contract or any other things with which you want to associate deposits received on the account. Reference ID is optional, so you can avoid passing your internal IDs to Sirius and keep mapping between your objects and Sirius accounts on your side.

## Creates an account

### Request

**Rest API:** `POST /api/accounts`

### Parameters

### Query Parameters

### Response

> POST /api/accounts 200 OK

```json
{
}
```

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