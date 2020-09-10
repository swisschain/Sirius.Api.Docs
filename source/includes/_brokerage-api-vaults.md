# Vaults

A Vault is a place where private keys of your blockchain wallets are kept. It generates public/private key pairs and signs transactions using these private keys. In order to create any broker-account, account, wallet, receive or send transactions in Sirius, you have to create a Vault first. There are two types of the Vault:

* Shared - run and managed by SwissChain. It uses software-only security to store the keys. It's used across all the customers which use this type of the Vault.
* Private - can be run and managed either by **SwissChain** or by the **Customer**. Either software-only or **IBM HSM** and **Hyper Protect Services** based implementation can be used. IBM HSM and Hyper Protect Services can be run either in the cloud or on-premise. IBM HSM and Hyper Protect Services is the only solution in the industry which is **FIPS 140-2 Level 4-certified**. [Contact](mailto:contact@swisschain.io) us to get help deploying of this type of the Vault.

We recommend using Shared Vault for the testing purposes and private for the production environment.

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
  "name": "My Secure Vault"
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
`vaultId` | `vault_id` | *number* | *path* | ID of the vault being update
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

**gRPC API:** `swisschain.sirius.api.vaults.Vaults/Search`

```json
GET /api/vaults?id=100010&name=My%20Vault&type=private&order=asc&cursor=100009&limit=3
```

```protobuf
swisschain.sirius.api.vaults.Vaults/Search

> Requets: (application/grpc)

message VaultSearchRequest {  
  google.protobuf.Int64Value id = 1;                            // 100010
  google.protobuf.StringValue name = 2;                         // "My Vault"
  repeated VaultType type = 3;                                  // Array<VaultType>
  swisschain.sirius.api.common.PaginationInt64 pagination = 4;  // Object
}

message PaginationInt64 {
  PaginationOrder order = 1;                                    // PaginationOrder.ASC
  google.protobuf.Int64Value cursor = 2;                        // 100009
  int32 limit = 3;                                              // 3
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`id` | `id` | *number, optional* | *query* | Exact ID of the vault to search
`name` | `name` | *string, optional* | *query* | Name of the vault to search
`type` | `type` | *Array<[VaultType](#VaultType)>, optional* | *query* | Zero or several vault types to search
`order` | `pagination.order` | *[Order](#order), optional* | *query* | Results sorting order
`cursor` | `pagination.cursor` | *number, optional* | *query* | Cursor to continue search from
`limit` | `pagination.limit` | *number, optional* | *query* | Max numbers of the items to return from the server

### Response

```json
GET /api/vaults?&name=Vault&type=private&order=asc&cursor=100009&limit=3

> Response: 200 (application/json) - success response

{
  "pagination": {
    "cursor": 100009,
    "count": 3,
    "order": "asc",
    "nextUrl": "/api/vaults?&name=Vault&type=private&order=asc&cursor=100012&limit=3"
  },
  "items": [
    {
      "id": 100010,
      "name": "My Vault",
      "type": "shared",
      "status": "online",
      "createdAt": "2020-08-27T22:08:56.976533Z",
      "updatedAt": "2020-08-27T22:08:56.976533Z"
    },
    {
      "id": 100011,
      "name": "My Secure Vault",
      "type": "private",
      "status": "online",
      "createdAt": "2020-08-27T22:08:56.976533Z",
      "updatedAt": "2020-08-27T22:08:56.976533Z"
    },
    {
      "id": 100012,
      "name": "Test Vault",
      "type": "shared",
      "status": "online",
      "createdAt": "2020-08-27T22:08:56.976533Z",
      "updatedAt": "2020-08-27T22:08:56.976533Z"
    }
  ]
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/Search

> Response: (application/grpc) - success response

message VaultSearchResponse {
  oneof result {
    VaultSearchResponseBody body = 1;                                   // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;           // NULL
  } 
}

message VaultSearchResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated VaultResponse items = 2;                                     // Array<Object>
}

message VaultUpdateResponseBody {
  VaultResponse vault = 1;                                              // Object
}

message PaginatedInt64Response {
  google.protobuf.Int64Value cursor = 1;                                // 100009
  int32 count = 2;                                                      // 3
  PaginationOrder order = 3;                                            // PaginationOrder.ASC
}

message VaultResponse {
  int64 id = 1;                                                         // 100010
  string name = 2;                                                      // "My Secure Vault"
  VaultType type = 3;                                                   // VaultType.PRIVATE
  VaultStatus status = 4;                                               // VaultStatus.OFFLINE
  google.protobuf.Timestamp created_at = 5;                             // "2020-08-24T21:43:02.6554641Z"
  google.protobuf.Timestamp updated_at = 6;                             // "2020-08-24T21:43:02.6554641Z"
}
```

[Paginated](#Pagination) list of

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the vault
`name` | `name` | *string* | Name of the vault
`type` | `type` | *[VaultType](#VaultType)* | Type of the vault
`status` | `status` | *[VaultStatus](#VaultStatus)* | Status of the vault
`createdAt` | `created_at` | *timestamp* | Date of the vault creation
`updatedAt` | `updated_at` | *timestamp* | Date of the vault update

## Vault API key creation

### Request

**Rest API:** `POST /api/vaults/{vaultId}/api-keys`

**gRPC API:** `swisschain.sirius.api.vaults.Vaults/AddApiKey`

```json
POST /api/vaults/100010/api-keys

> Request: (application/json)

x-request-id: d5125d2d4d9445efa5414a3926ab00a7

{
  "name": "API key for my vault",
  "expiresAt": "2020-09-24T21:43:02.6554641Z"
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/AddApiKey

> Requets: (application/grpc)

message VaultAddApiKeyRequest {
  string request_id = 1;                        // d5125d2d4d9445efa5414a3926ab00a7
  int64 vault_id = 2;                           // 100010
  string name = 3;                              // "API key for my vault"
  google.protobuf.Timestamp expires_at = 4;     // "2020-09-24T21:43:02.6554641Z"
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`X-Request-ID` | - | *string* | *header* | Unique ID of the request
 - | `request_id` | *string* | - | Unique ID of the request
`vaultId` | `vault_id` | *number* | *path* | ID of the vault being update
`name` | `name` | *string* | *body* | Name of the vault to set
`expiresAt` | `expires_at` | *timestamp* | *body* | Date of the API key expiration

### Response

```json
POST /api/vaults/100010/api-keys

> Response: 200 (application/json) - success response

{
  "id": 100014,
  "vaultId": 100010,
  "name": "API key for my vault",
  "expiresAt": "2020-09-24T21:43:02.6554641Z",
  "issuedAt": "2020-08-24T21:43:02.6554641Z",
  "isRevoked": false
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/AddApiKey

> Response: (application/grpc) - success response

message VaultAddApiKeyResponse {
  oneof result {
    VaultAddApiKeyResponseBody body = 1;                            // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;       // NULL
  } 
}

message VaultAddApiKeyResponseBody {
  VaultApiKeyResponse vault_api_key = 1;                            // Object
}

message VaultApiKeyResponse {
  int64 id = 1;                                                     // 100014
  int64 vault_id = 2;                                               // 100010
  string name = 3;                                                  // "API key for my vault"
  google.protobuf.Timestamp expires_at = 4;                         // "2020-09-24T21:43:02.6554641Z"
  google.protobuf.Timestamp issued_at = 5;                          // "2020-08-24T21:43:02.6554641Z"
  bool is_revoked = 6;                                              // FALSE
}
```

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the vault API key
`vaultId` | `vault_id` | *number* | ID of the vault
`name` | `name` | *string* | Name of the vault
`expiresAt` | `expires_at` | *timestamp* | Date of the API key expiration
`issuedAt` | `issued_at` | *timestamp* | Date of the API key issuance
`isRevoked` | `is_revoked` | *bool* | Flag indicating if the API key is revoked

## Vault API keys search

### Request

**Rest API:** `GET /api/vaults/{vaultId}/api-keys`

**gRPC API:** `swisschain.sirius.api.vaults.Vaults/SearchApiKeys`

```json
GET /api/vaults/100010/api-keys?id=100014&name=API%20key%20for%20my%20vault&isRevoked=false&order=asc&cursor=100013&limit=3
```

```protobuf
swisschain.sirius.api.vaults.Vaults/SearchApiKeys

> Requets: (application/grpc)

message VaultSearchApiKeyRequest {  
  google.protobuf.Int64Value id = 1;                            // 100014
  google.protobuf.StringValue name = 2;                         // "API key for my vault"
  int64 vault_id = 4;                                           // 100010
  google.protobuf.BoolValue is_revoked = 5;                     // false
  swisschain.sirius.api.common.PaginationInt64 pagination = 6;  // Object
}

message PaginationInt64 {
  PaginationOrder order = 1;                                    // PaginationOrder.ASC
  google.protobuf.Int64Value cursor = 2;                        // 100013
  int32 limit = 3;                                              // 3
}
```

REST name | gRPC name | type | REST placement | description 
--------- | --------- | ---- | -------------- | -----------
`vaultId` | `vault_id` | *number* | *path* | Exact ID of the vault to search
`id` | `id` | *number, optional* | *query* | Exact ID of the vault API key to search
`name` | `name` | *string, optional* | *query* | Name of the vault API key to search
`isRevoked` | `is_revoked` | *bool, optional* | *query* | Flag indicating if only revoked or only not revoked vault API keys should be searched
`order` | `pagination.order` | *[Order](#order), optional* | *query* | Results sorting order
`cursor` | `pagination.cursor` | *number, optional* | *query* | Cursor to continue search from
`limit` | `pagination.limit` | *number, optional* | *query* | Max numbers of the items to return from the server

### Response

```json
GET /api/vaults/100010/api-keys?name=API%20key&isRevoked=false&order=asc&cursor=100013&limit=3

> Response: 200 (application/json) - success response

{
  "pagination": {
    "cursor": 100009,
    "count": 3,
    "order": "asc",
    "nextUrl": /api/vaults/100010/api-keys?name=API%20key&isRevoked=false&order=asc&cursor=100016&limit=3
  },
  "items": [
    {
      "id": 100014,
      "vaultId": 100010,
      "name": "API key for my vault",
      "expiresAt": "2021-09-10T20:26:22.845Z",
      "issuedAt": "2020-09-10T20:26:22.845Z",
      "isRevoked": false
    },
    {
      "id": 100015,
      "vaultId": 100010,
      "name": "Another one API key for my vault",
      "expiresAt": "2021-09-10T20:26:22.845Z",
      "issuedAt": "2020-09-10T20:26:22.845Z",
      "isRevoked": false
    },
    {
      "id": 100016,
      "vaultId": 100010,
      "name": "One more API key for my vault",
      "expiresAt": "2021-09-10T20:26:22.845Z",
      "issuedAt": "2020-09-10T20:26:22.845Z",
      "isRevoked": false
    }
  ]
}
```

```protobuf
swisschain.sirius.api.vaults.Vaults/SearchApiKeys

> Response: (application/grpc) - success response

message VaultSearchApiKeyResponse {
  oneof result {
    VaultSearchApiKeyResponseBody body = 1;                             // Object
    swisschain.sirius.api.common.ErrorResponseBody error = 2;           // NULL
  } 
}

message VaultSearchApiKeyResponseBody {
  swisschain.sirius.api.common.PaginatedInt64Response pagination = 1;   // Object
  repeated VaultApiKeyResponse items = 2;                               // Array<Object>
}

message VaultApiKeyResponse {
  int64 id = 1;                                                         // 100016
  int64 vault_id = 2;                                                   // 100010
  string name = 3;                                                      // "One more API key for my vault"
  google.protobuf.Timestamp expires_at = 4;                             // "2021-09-10T20:26:22.845Z"
  google.protobuf.Timestamp issued_at = 5;                              // "2020-09-10T20:26:22.845Z"
  bool is_revoked = 6;                                                  // FALSE
}
```

[Paginated](#Pagination) list of

REST name | gRPC name | type | description
--------- | --------- | ---- | -----------
`id` | `id` | *number* | ID of the vault API key
`vaultId` | `vault_id` | *number* | ID of the vault
`name` | `name` | *string* | Name of the vault API key
`expiresAt` | `expires_at` | *timestamp* | Date of the API key expiration
`issuedAt` | `issued_at` | *timestamp* | Date of the API key issuance
`isRevoked` | `is_revoked` | *bool* | Flag indicating if the vault API key is revoked

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