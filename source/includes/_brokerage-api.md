# Blockchains

We distinguish *Blockchain*, *Protocol*, and *Network Type*.

*Blockchain* is not just Bitcoin or Ethereum, it's a particular network. For example Bitcoin MainNet, Ethereum Ropsten are blockchains, whilst Bitcoin and Ethereum are protocols which are used by these blockchains. Networks can be one of the types - `private`, `test`, `public`. So Blockchain has a protocol and network type.

*Blockchain* is an "instance" of the protocol - Ethereum Ropsten, Bitcoin Mainnet, Ethereum Mainnet.

Protocol is a set of rules on how blockchain nodes interact with each other - `Bitcoin`, `Ethereum`, `Ripple`.

Network type can be `private`, `test`, or `public`.

## Search blockchains

### Request

**Rest API:** `GET /api/blockchains`

### Query Parameters

name | type | description | example
---- | ---- | ----------- | -------
`id` | *optional*, *string* | Exact ID of the blockchain to search | bitcoin-private
`name` | *optional*, *string* | Text to search in the name of the blockchain | Bitcoin private network
`protocolCode` | *optional*, *string* | Exact ode of the protocol to search | bitcoin
`protocolName` | *optional*, *string* | Text to search in the name of the protocol | Bitcoin
`networkType` | *optional*, *[NetworkType](#networkType)* | List of the network types to search. Repeat the parameter to specify multiple items | private
`order` | *optional*, *[Order](#order)* | Result items sorting order | asc
`cursor` | *optional*, *string* | Cursor to continue the search | stellar-test
`limit` | *optional*, *number* | Maximum number of items to return in the search results | 10

### Response

Paginated array of the blockchains:

name | type | description | example
---- | ---- | ----------- | -------
`id` | string | Unique identifier. | `bitcoin-private`
`tenantId` | *optional*, *string* | Id of the tenant that owns the blockchain | 100000
`name` | *string* | Name of the blockchain | `bitcoin-private`
`networkType` | *[NetworkType](#networkType)* | Type of the network
`protocol` | *[BlockchainProtocol](#BlockchainProtocol)* | Protocol of the blockchain.
`latestBlockNumber` | *number* | Number of the latest block | 1567432

> GET /api/blockchains 200 OK

```json
{
  "pagination": {
    "cursor": null,
    "count": 3,
    "order": "asc",
    "nextUrl": "/api/blockchains?Order=Asc&Cursor=stellar-test&Limit=3"
  },
  "items": [
    {
      "id": "bitcoin-private",
      "protocol": {
        "code": "bitcoin",
        "name": "Bitcoin",
        "startBlockNumber": 0,
        "requirements": {
          "publicKey": true
        },
        "capabilities": {
          "destinationTag": {
            "number": null,
            "text": null
          }
        },
        "doubleSpendingProtectionType": "coins"
      },
      "tenantId": null,
      "name": "Bitcoin private network",
      "networkType": "private",
      "latestBlockNumber": 5899
    },
    {
      "id": "ethereum-ropsten",
      "protocol": {
        "code": "ethereum",
        "name": "Ethereum",
        "startBlockNumber": 0,
        "requirements": {
          "publicKey": false
        },
        "capabilities": {
          "destinationTag": {
            "number": null,
            "text": null
          }
        },
        "doubleSpendingProtectionType": "nonce"
      },
      "tenantId": null,
      "name": "Ethereum test network",
      "networkType": "test",
      "latestBlockNumber": 8450781
    },
    {
      "id": "stellar-test",
      "protocol": {
        "code": "stellar",
        "name": "Stellar",
        "startBlockNumber": 1,
        "requirements": {
          "publicKey": false
        },
        "capabilities": {
          "destinationTag": {
            "number": {
              "min": 1,
              "max": 9223372036854776000,
              "name": "Memo ID"
            },
            "text": {
              "maxLength": 28,
              "name": "Memo text"
            }
          }
        },
        "doubleSpendingProtectionType": "nonce"
      },
      "tenantId": null,
      "name": "Stellar test network",
      "networkType": "test",
      "latestBlockNumber": 133093
    }
  ]
}
```

## Get latest block

### Request

**Rest API:** `GET /api/blockchains/{id}/latest-block`

### Parameters

name | type | description | example
---- | ---- | ----------- | -------
`id` | *string* | Exact ID of the blockchain to search | bitcoin-private

### Response

The latest block of the specified blockchain:

name | type | description | example
---- | ---- | ----------- | -------
`blockNumber` | *number* | Number of the latest block. | 5933

> GET /api/blockchains/{id}/latest-block 200 OK

```json
{
  "blockNumber": 5933
}
```

# Assets

## Search assets

### Request

**Rest API:** `GET /api/assets`

### Parameters

### Query Parameters

name | type | description | example
---- | ---- | ----------- | -------
`id` | *optional*, *number* | ID of the asset to search | 100553
`blockchainId` | *optional*, *string* | Exact blockchain id to search | bitcoin-private
`symbol` | *optional*, *string* | Text to search in the asset symbol | BTC
`address` | *optional*, *string* | Exact address to search | 0xC1701AbD559FC263829CA3917d03045F95b5224A
`accuracy` | *optional*, *number* | Exact accuracy to search | 8
`order` | *optional*, *[Order](#order)* | Result items sorting order | asc
`cursor` | *optional*, *string* | Cursor to continue the search | stellar-test
`limit` | *optional*, *number* | Maximum number of items to return in the search results | 10

### Response

Paginated array of the assets:

name | type | description | example
---- | ---- | ----------- | -------
`id` | *number* | ID of the asset to search | 100003
`blockchainId` | *string* | Exact blockchain id to search | bitcoin-private
`symbol` | *string* | Text to search in the asset symbol | BTC
`address` | *optional*, *string* | Exact address to search | 0xC1701AbD559FC263829CA3917d03045F95b5224A
`accuracy` | *number* | Exact accuracy to search | 8

> GET /api/assets 200 OK

```json
{
  "pagination": {
    "cursor": null,
    "count": 3,
    "order": "asc",
    "nextUrl": "/api/assets?Symbol=BTC&Order=Asc&Cursor=100110&Limit=3"
  },
  "items": [
    {
      "id": 100003,
      "blockchainId": "ethereum-ropsten",
      "symbol": "WBTC",
      "address": "0x3DFf0dCE5fc4B367Ec91d31DE3837cf3840c8284",
      "accuracy": 8
    },
    {
      "id": 100085,
      "blockchainId": "ethereum-ropsten",
      "symbol": "sBTC",
      "address": "0xC1701AbD559FC263829CA3917d03045F95b5224A",
      "accuracy": 18
    },
    {
      "id": 100110,
      "blockchainId": "ethereum-ropsten",
      "symbol": "TBTC",
      "address": "0xa609f2c9C5B7873F353C15D4ef6e151D14db69CC",
      "accuracy": 18
    }
  ]
}
```

# Vaults

## Creates a vault

### Request

**Rest API:** `POST /api/vaults`

### Parameters

### Query Parameters

### Response

> GET /api/vaults 200 OK

```json
{
}
```

## Updates a vault

### Request

**Rest API:** `PUT /api/vaults/{vaultId}`

### Parameters

### Query Parameters

### Response

> PUT /api/vaults/{vaultId} 200 OK

```json
{
}
```

## Searches vaults

### Request

**Rest API:** `GET /api/vaults`

### Parameters

### Query Parameters


### Response

> POST /api/vaults 200 OK

```json
{
}
```

## Creates a vault API key

### Request

**Rest API:** `POST /api/vaults/{vaultId}/api-keys`

### Parameters

### Query Parameters

### Response

> POST /api/vaults/{vaultId}/api-keys 200 OK

```json
{
}
```

## Searches vault API keys

### Request

**Rest API:** `GET /api/vaults/{vaultId}/api-keys`

### Parameters

### Query Parameters

### Response

> GET /api/vaults/{vaultId}/api-keys 200 OK

```json
{
}
```

## Gets the vault API key token

### Request

**Rest API:** `GET /api/vaults/{vaultId}/api-keys/{apiKeyId}/token`

### Parameters

### Query Parameters

### Response

> GET /api/vaults/{vaultId}/api-keys/{apiKeyId}/token 200 OK

```json
{
}
```

## Revokes the vault API key token

### Request

**Rest API:** `POST /api/vaults/{vaultId}/api-keys/{apiKeyId}/revoke`

### Parameters

### Query Parameters

### Response

> POST /api/vaults/{vaultId}/api-keys/{apiKeyId}/revoke 200 OK

```json
{
}
```

# Broker accounts

## Creates a broker account

### Request

**Rest API:** `POST /api/broker-accounts`

### Parameters

### Query Parameters

### Response

> POST /api/broker-accounts 200 OK

```json
{
}
```

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

# Accounts

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

# Deposits

## Searches deposits

### Request

**Rest API:** `GET /api/deposits`

### Parameters

### Query Parameters

### Response

> GET /api/deposits 200 OK

```json
{
}
```

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

# Withdrawals

## Starts a withdrawal

### Request

**Rest API:** `POST /api/withdrawals`

### Parameters

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

# Data structures

## Order (enum)

Order of the sorting

+ `asc` - Ascending sorting order
+ `desc` - Descending sorting order

## NetworkType (enum)

Type of a blockchain network

+ `private` - Private blockchain.
+ `test` - Public test blockchain.
+ `public` - Public mainnet blockchain.

## BlockchainProtocol (object)

+ `code` (*string*) - Code of the protocol. (eg. "bitcoin").
+ `name` (*string*) - Name of the protocol. (eg. "Bitcoin").
+ `startBlockNumber` (*number*) - Number of the first block in the blockchain. Usually 0 or 1.
+ `requirements` (*object*) - Requirements of the protocol.
    + `publicKey` (*boolean*) - Indicates, if source address public key is required to build a transaction. (eg. `true`).
+ `capabilities` (*object*) - Capabilities of the protocol.
    + `destinationTag` (*optional*, *object*) - Capabilities of the protocol related to the destination tag. `null` if destination tag is not supported.
        + `number` (*optional*, *object*) - Capabilities of the protocol related to the `Number` [TagType](#TagType). `null` if `Number` tag is not suppoerted.
            + `min` (*number*) - Minimum number supported by the `Number` destination tag. (eg. 1).
            + `max` (*number*) - Maximum number supported by the `Number` destination tag. (eg. 9223372036854776000).
            + `name` (*string*) - Name of the `Number` tag field which is accepted in the given blockchain. (eg. "Memo ID").
        + `text` (*optional*, *object*) - Capabilities of the protocol related to the `Text` [TagType](#TagType). `null` if the `Text` tag is not supported.
            + `maxLength` (*number*) - Mximum length of the `Text` destination tag. (eg. 28).
            + `name` (*string*) - Name of the `Text` tag field which is accepted in the given blockchain. (eg. "Memo text").
        + `doubleSpendingProtectionType` (*[DoubleSpendingProtectionType](#DoubleSpendingProtectionType)*) - Type of the protection agains funds double spending (eg. `coins`).

## TagType (enum)

Tag type of the account. Tag is an additional information associated with particular account, 
which can be specified in the blockchain transaction. This allows to use single blockchain 
address to handle many accounts on it. This feature is available only on some blockchains: 
(Ripple, Stellar, Eos...). In some blockchains different types of the tag are supported
and you have to specify exact type in this case.

+ `text` - text tag
+ `number` - numeric tag

## DoubleSpendingProtectionType (enum)

Each blockchain use some protection mechanism agains funds double-spending. Sirius distinguish next types:

+ `coins` - Coins are used to transfer funds. (eg. Bitcoin, Dash)
+ `nonce` - Nonce number (sequence) is used to distinguish outgoing transactions. (eg. Ethereum, Stellar)

## BrokerAccountState (enum)

State of the broker account

+ `creating` - The broker account is not fully created yet, you need to wait a bit, while Sirius complete its creation. You can create accounts within it, but not receive or send transfers.
+ `active` - The broker account is active and can be used as usual.
+ `blocked` - The broker account is blocked - processing of all deposits and withdrawals is on hold.

## AccountState (enum)

State of the account

+ `creating` - The account is not fully created yet, you need to wait a bit, while Sirius complete its creation. You can't receive or send transfers.
+ `active` - The account is active and can be used as usual.
+ `blocked` - The account is blocked - processing of all deposits and withdrawals is on hold.

## BrokerAccountDetails (object)

Details of the broker account

+ `id` (*number*) - Unique ID.
+ `brokerAccountId` (*number*) - Broker account ID.
+ `createdAt` (*timestamp*)- Creation time of the details.
+ `address` (*string*) - Blockchain address of the details.
+ `blockchainId` (*string*) - Blockchain ID of the details.

## BrokerAccountDetailsBrief (object)

Brief broker account details

+ `id` (*number*) - Unique ID.
+ `address` (*string*) - Blockchain address of the details.

## AccountDetails (object)

Details of the account

+ `id` (*number*) - Unique ID.
+ `accountId` (*number*) - Account ID.
+ `createdAt` (*timestamp*) - Creation time of the details.
+ `address` (*string*) - Blockchain address of the details.
+ `blockchainId` (*string*) - Blockchain ID of the details.
+ `tag` (*optional*, *string*) - Tag of the details.
+ `tagType` (*optional*, *[TagType](#/data-structures/0/tag-type)*) - Type of the details tag.

## AccountDetailsBrief (object)

Brief details of the account

+ `id` (*number*) - Unique ID.
+ `address` (*string*) - Blockchain address of the details.
+ `tag` (*optional*, *string*) - Tag of the details.
+ `tagType` (*optional*, *[TagType](#TagType*) - Type of the details tag.

## BrokerAccountBalances (object)

Balances of the broker account in a particular asset. We distinguish 4 different balances
for each asset in each broker account.

+ `id` (*number*) - Unique ID.
+ `brokerAccountId` (*number*) - Broker account ID.
+ `assetId` (*number*) - Asset ID.
+ `ownedBalance` (*number*) - Balance owned by the broker.
+ `availableBalance` (*number*) - Balance available for a withdrawal.
+ `pendingBalance` (*number*) - Balance which is being deposited, but not completely transferred yet.
+ `reservedBalance` (*number*) - Balance which is being withdrawn, but not completely transferred yet.
+ `createdAt` (*timestamp*) - Creation time of the balances.
+ `updatedAt` (*timestamp*) - The update time of the balances.
