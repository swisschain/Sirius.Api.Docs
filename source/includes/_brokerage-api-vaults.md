# Vaults

## Creates a vault

### Request

**Rest API:** `POST /api/vaults`

**gRPC API:** `swisschain.sirius.api.vaults.Vaults/Create`

```json
POST /api/vaults

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "name": "My Vault",
  "type": "private"
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/Create

> Requets: (application/grpc)

message VaultCreateRequest {
  string request_id = 1;    // 1a5c0b3d15494ec8a390fd3b22d757d6
  string name = 2;          // "My Vault"
  VaultType type = 3;       // VaultType.PRIVATE
}
```

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`X-Request-ID` | - | *string, header* | Uniqueu ID of the request | 1a5c0b3d15494ec8a390fd3b22d757d6
 - | `request_id` | *string* | Uniqueu ID of the request | 1a5c0b3d15494ec8a390fd3b22d757d6
`name` | `name` | *string* | Name of the vault | My Vault
`type` | `type` | *[VaultType](#VaultType)* | Type of the vault | private

### Response

```json
POST /api/vaults 

> Response: 200 (application/json) - success response

{
  "id": 100010,
  "name": "My Vault",
  "type": "private",
  "status": "offline",
  "createdAt": "2020-08-24T21:43:02.6554641Z",
  "updatedAt": "2020-08-24T21:43:02.6554641Z"
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/Create

> Response: (application/grpc) - success response

message VaultCreateResponse {
    oneof result {
      VaultCreateResponseBody body = 1;                         // Object
      swisschain.sirius.api.common.ErrorResponseBody error = 2; // NULL
    } 
}

message VaultCreateResponseBody {
  VaultResponse vault = 1;                                      // Object
}

message VaultResponse {
  int64 id = 1;                                                 // 100010
  string name = 2;                                              // "My Vault"
  VaultType type = 3;                                           // VaultType.PRIVATE
  VaultStatus status = 4;                                       // VaultStatus.OFFLINE
  google.protobuf.Timestamp created_at = 5;                     // "2020-08-24T21:43:02.6554641Z"
  google.protobuf.Timestamp updated_at = 6;                     // "2020-08-24T21:43:02.6554641Z"
}
```

REST name | gRPC name | type | description | example
--------- | --------- | ---- | ----------- | -------
`id` | `id` | *number* | ID of the vault | 100010
`name` | `name` | *string* | Name of the vault | My Vault
`type` | `type` | *[VaultType](#VaultType)* | Type of the vault | private
`status` | `status` | *[VaultStatus](#VaultStatus)* | Status of the vault | offline
`createdAt` | `created_at` | *timestamp* | Date of the vault creation | 2020-08-24T21:43:02.6554641Z
`updatedAt` | `updated_at` | *timestamp* | Date of the vault update | 2020-08-24T21:43:02.6554641Z

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