# Vaults

## Vault creation

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

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`name` | `name` | *string* | *body* | Name of the vault
`type` | `type` | *[VaultType](#VaultType)* | *body* | Type of the vault

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

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the vault
`name` | `name` | *string* | Name of the vault
`type` | `type` | *[VaultType](#VaultType)* | Type of the vault
`status` | `status` | *[VaultStatus](#VaultStatus)* | Status of the vault
`createdAt` | `created_at` | *timestamp* | Date of the vault creation
`updatedAt` | `updated_at` | *timestamp* | Date of the vault update

## Vault update

### Request

**Rest API:** `PUT /api/vaults/{vaultId}`

**gRPC API:** `swisschain.sirius.api.vaults.Vaults/Update`

```json
PUT /api/vaults/100010

> Request: (application/json)

x-request-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "name": "My Vault"
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/Update

> Requets: (application/grpc)

message VaultUpdateRequest {
  string request_id = 1;    // 1a5c0b3d15494ec8a390fd3b22d757d6
  int64 vault_id = 2;       // 100010
  string name = 3;          // "My Secure Vault"
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`vaultId` | `vault_id` | *number* | *route* | ID of the vault being update
`name` | `name` | *string* | *body* | Name of the vault to set

### Response

```json
PUT /api/vaults/100010

> Response: 200 (application/json) - success response

{
  "id": 100010,
  "name": "My Secure Vault",
  "type": "private",
  "status": "offline",
  "createdAt": "2020-08-24T21:43:02.6554641Z",
  "updatedAt": "2020-08-24T21:43:02.6554641Z"
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/Update

> Response: (application/grpc) - success response

message VaultUpdateResponse {
  oneof result {
    VaultUpdateResponseBody body = 1;                           // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;   // NULL
  } 
}

message VaultUpdateResponseBody {
  VaultResponse vault = 1;                                      // Object
}

message VaultResponse {
  int64 id = 1;                                                 // 100010
  string name = 2;                                              // "My Secure Vault"
  VaultType type = 3;                                           // VaultType.PRIVATE
  VaultStatus status = 4;                                       // VaultStatus.OFFLINE
  google.protobuf.Timestamp created_at = 5;                     // "2020-08-24T21:43:02.6554641Z"
  google.protobuf.Timestamp updated_at = 6;                     // "2020-08-24T21:43:02.6554641Z"
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the vault
`name` | `name` | *string* | Name of the vault
`type` | `type` | *[VaultType](#VaultType)* | Type of the vault
`status` | `status` | *[VaultStatus](#VaultStatus)* | Status of the vault
`createdAt` | `created_at` | *timestamp* | Date of the vault creation
`updatedAt` | `updated_at` | *timestamp* | Date of the vault update

## Vaults search

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

## Vault API key creation

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

## Vault API keys search

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

## Vault API key token obtaining

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

## Vault API key token revocation

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