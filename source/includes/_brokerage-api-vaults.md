# Vaults

## Creates a vault

### Request

**Rest API:** `POST /api/vaults`

#### Headers

name | type | description | example
---- | ---- | ----------- | -------
`X-Request-ID` | *string* | Uniqueu ID of the request | 1a5c0b3d15494ec8a390fd3b22d757d6

#### Body

name | type | description | example
---- | ---- | ----------- | -------
`name` | *string* | Name of the vault | production
`type` | *[VaultType](#VaultType)* | Type of the vault | private

> 

```json
x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "name": "production",
  "type": "private"
}
```

### Response

> POST /api/vaults Response: 200 OK

```json
{
  "id": 100010,
  "name": "production",
  "type": "private",
  "status": "offline",
  "createdAt": "2020-08-24T21:43:02.6554641Z",
  "updatedAt": "2020-08-24T21:43:02.6554641Z"
}
```

## Updates a vault

### Request

**Rest API:** `PUT /api/vaults/{vaultId}`

#### Parameters

#### Query Parameters

### Response

> PUT /api/vaults/{vaultId} 200 OK

```json
{
}
```

## Searches vaults

### Request

**Rest API:** `GET /api/vaults`

#### Parameters

#### Query Parameters


### Response

> POST /api/vaults 200 OK

```json
{
}
```

## Creates a vault API key

### Request

**Rest API:** `POST /api/vaults/{vaultId}/api-keys`

#### Parameters

#### Query Parameters

### Response

> POST /api/vaults/{vaultId}/api-keys 200 OK

```json
{
}
```

## Searches vault API keys

### Request

**Rest API:** `GET /api/vaults/{vaultId}/api-keys`

#### Parameters

#### Query Parameters

### Response

> GET /api/vaults/{vaultId}/api-keys 200 OK

```json
{
}
```

## Gets the vault API key token

### Request

**Rest API:** `GET /api/vaults/{vaultId}/api-keys/{apiKeyId}/token`

#### Parameters

#### Query Parameters

### Response

> GET /api/vaults/{vaultId}/api-keys/{apiKeyId}/token 200 OK

```json
{
}
```

## Revokes the vault API key token

### Request

**Rest API:** `POST /api/vaults/{vaultId}/api-keys/{apiKeyId}/revoke`

#### Parameters

#### Query Parameters

### Response

> POST /api/vaults/{vaultId}/api-keys/{apiKeyId}/revoke 200 OK

```json
{
}
```