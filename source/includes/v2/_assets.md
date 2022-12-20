# Assets

Sirius provides the set of crypto assets available to use.
You can get notifications about a deposit or initiate a withdrawal in any asset presented in this set.
The asset is a generic model which represents a native asset, fungible token or a non-fungible token.
It's possible that in the different blockchains assets with the same symbol and/or address are existed, so you have to rely only on the asset ID if you need to deterministically identify it.
If you've received a deposit of an asset which is not supported by Sirius yet, you can add this particular asset to your personal list by specifying all needed asset parameters.

**Rest API:** `/api/v2/assets`

**gRPC API:** `swisschain.sirius.api.v2.assets`

## Search

Returns assets by search parameters.

gRPC API example:
```csharp
var request = new AssetSearchRequest
{
    Id = 10001,
    BlockchainId = "ethereum",
    Symbol = "USDC",
    TokenId = null,
    Address = "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    Accuracy = 6,
    Pagination = new PaginationInt64
    {
        Cursor = null,
        Limit = 100,
        Order = PaginationOrder.Asc
    }
};

var response = client.AssetsV2.SearchAsync(request);
```

REST API example:
```json
GET /api/v2/assets?blockchainId=bitcoin&symbol=BTC
```

### Request

| REST name      | gRPC name       | type                                                                  | description                                          | example                                    |
|----------------|:----------------|-----------------------------------------------------------------------|------------------------------------------------------|--------------------------------------------|
| `id`           | `id`            | *optional*, *long*                                                    | ID of the asset to search                            | 100553                                     |
| `blockchainId` | `blockchain_id` | *optional*, *string*                                                  | Exact blockchain id to search                        | bitcoin-private                            |
| `symbol`       | `symbol`        | *optional*, *string*                                                  | Text to search in the asset symbol                   | BTC                                        |
| `tokenId`      | `token_id`      | *optional*, *string*                                                  | Text to search in the asset symbol                   | BTC                                        |
| `address`      | `address`       | *optional*, *string*                                                  | Exact address to search                              | 0xC1701AbD559FC263829CA3917d03045F95b5224A |
| `accuracy`     | `accuracy`      | *optional*, *int*                                                     | Exact accuracy to search                             | 8                                          |
| `pagination`   | ---             | *optional*, *[Pagination](#api-usage-pagination-request)*             | pagination for gRPC API                              |                                            |
| `order`        | `order`         | *optional*, *[PaginationOrder](#api-usage-pagination-request)*, enum  | *REST* Descending or ascending sort of entries       |                                            |
| `limit`        | `limit`         | *optional*, *[PaginationLimit](#api-usage-pagination-request)*, int   | *REST* Maximum number of entries to fetch            |                                            |
| `cursor`       | `cursor`        | *optional*, *[PaginationCursor](#api-usage-pagination-request)*, long | *REST* ID of anchor entity to start pagination from  |                                            |

### Response

*[Paginated](#api-usage-pagination-response)* array of the [assets](#assets-models-asset).

| Error Code         | Meaning                                            |
|--------------------|----------------------------------------------------|
| UNKNOWN            | An unspecified error code received.                |
| INVALID_PARAMETERS | Your request contains invalid parameters.          |
| DOMAIN_PROBLEM     | An error occurred while processing request.        |
| TECHNICAL_PROBLEM  | We had a problem with our server. Try again later. |

## AddAttributes

Adds asset attributes.

gRPC API example:
```csharp
var request = new AssetAddAttributesRequest
{
    IdempotencyId = "AD1D5199-1D1F-4B2C-AE6C-DED0DAAC4D34",
    AssetId = 10001,
    Aml = new AmlAssetAttributesCreateInfo
    {
        Chainalysis = new AmlChainalysisAssetAttributesInfo
        {
            Symbol = "USDC",
            TransferFormat = ChainalysisTransferFormat.HashAndAddress,
            IsCheckDepositsEnabled = true,
            IsCheckWithdrawalsEnabled = true,
            IsNotifyWithdrawalsEnabled = true
        },
        MerkleScience = new AmlMerkleScienceAssetAttributesInfo
        {
            Symbol = "USDC",
            Currency = 14,
            IsDisabled = false
        }
    },
    Brokerage = new BrokerageAssetAttributesCreateInfo
    {
        MinDepositThreshold = 1,
        MinWithdrawalAmount = 1
    }
};

var response = client.AssetsV2.AddAttributes(request);
```

REST API example:
```json
POST /api/v2/assets

> Request: (application/json)

> Headers:  
X-Idempotency-ID: AD1D5199-1D1F-4B2C-AE6C-DED0DAAC4D34
 
> Body:
{
  "assetId":123122,
  "brokerage":{
    "minDepositThreshold":9,
    "minWithdrawalAmount":144
  },
  "aml":{
    "chainalysis":{
      "symbol":"MANA",
      "isCheckWithdrawalsEnabled":true,
      "isCheckDepositsEnabled":true,
      "isNotifyWithdrawalsEnabled":true,
      "transferFormat":"hashAndIndex"
    },
    "merkleScience":{
      "symbol":"MANA",
      "currency":3,
      "isDisabled":true
    }
  }
}
```

### Request

| REST name          | gRPC name       | type                                                                                      | description                | example                                |
|--------------------|-----------------|-------------------------------------------------------------------------------------------|----------------------------|----------------------------------------|
| `X-Idempotency-ID` | ---             | *string*                                                                                  | *header*                   | Unique ID of the request               |
| ---                | `IdempotencyId` | *string*                                                                                  | Request unique identifier  | "AD1D5199-1D1F-4B2C-AE6C-DED0DAAC4D34" |
| `assetId`          | `asset_id`      | *long*                                                                                    | Asset identifier in Sirius | 10001                                  |
| `aml`              | `aml`           | *[AmlAssetAttributesCreateInfo](#assets-models-amlassetattributescreateinfo)*             | AML asset attributes       |                                        |
| `brokerage`        | `brokerage`     | *[BrokerageAssetAttributesCreateInfo](#assets-models-brokerageassetattributescreateinfo)* | Brokerage asset attributes |                                        |

### Response

| REST name                        | gRPC name                       | type               | description                           | example |
|----------------------------------|---------------------------------|--------------------|---------------------------------------|---------|
| `BrokerageAssetAttributesInfoId` | `brokerage_asset_attributes_id` | *required* *long*  | Brokerage asset attributes identifier | 30002   |
| `AmlAssetAttributesInfoId`       | `aml_asset_attributes_id`       | *long*             | AML asset attributes identifier       | 40002   |

| Error Code         | Meaning                                            |
|--------------------|----------------------------------------------------|
| UNKNOWN            | An unspecified error code received.                |
| INVALID_PARAMETERS | Your request contains invalid parameters.          |
| DOMAIN_PROBLEM     | An error occurred while processing request.        |
| TECHNICAL_PROBLEM  | We had a problem with our server. Try again later. |

## Models

### Asset 

| REST name      | gRPC name       | type                                                                          | description                           | example                                    |
|----------------|-----------------|-------------------------------------------------------------------------------|---------------------------------------|--------------------------------------------|
| `id`           | `id`            | *long*                                                                        | ID of the asset                       | 100003                                     |
| `blockchainId` | `blockchain_id` | *string*                                                                      | Exact blockchain id                   | bitcoin-private                            |
| `symbol`       | `symbol`        | *string*                                                                      | Asset symbol                          | BTC                                        |
| `tokenId`      | `token_id`      | *string*                                                                      | Token ID                              | 101                                        |
| `address`      | `address`       | *string*                                                                      | Exact address                         | 0xC1701AbD559FC263829CA3917d03045F95b5224A |
| `accuracy`     | `accuracy`      | *int*                                                                         | Exact accuracy                        | 8                                          |
| `name`         | `name`          | *string*                                                                      | The name of token                     | Toy                                        |
| `metadataUrl`  | `metadata_url`  | *string*                                                                      | The token metadata URL                | ipfs://metadata.json                       |
| `type`         | `type`          | *string*                                                                      | The blockchain-specific type of token | ERC-20                                     |
| `aml`          | `aml`           | *[AmlAssetAttributesInfo](#assets-models-amlassetattributesinfo)*             | The AML attributes                    |                                            |
| `brokerage`    | `brokerage`     | *[BrokerageAssetAttributesInfo](#assets-models-brokerageassetattributesinfo)* | The brokerage attributes              |                                            |

### AmlAssetAttributesCreateInfo

| REST name       | gRPC name        | type                                                                                        | description              | example |
|-----------------|------------------|---------------------------------------------------------------------------------------------|--------------------------|---------|
| `chainalysis`   | `chainalysis`    | *[AmlChainalysisAssetAttributesInfo](#assets-models-amlchainalysisassetattributesinfo)*     | Chainalysis attributes   |         |
| `merkleScience` | `merkle_science` | *[AmlMerkleScienceAssetAttributesInfo](#assets-models-amlmerklescienceassetattributesinfo)* | MerkleScience attributes |         |

### AmlAssetAttributesInfo

| REST name       | gRPC name        | type                                                                                        | description              | example |
|-----------------|------------------|---------------------------------------------------------------------------------------------|--------------------------|---------|
| `id`            | `id`             | *long*                                                                                      | ID of the info           | 300001  |
| `chainalysis`   | `chainalysis`    | *[AmlChainalysisAssetAttributesInfo](#assets-models-amlchainalysisassetattributesinfo)*     | Chainalysis attributes   |         |
| `merkleScience` | `merkle_science` | *[AmlMerkleScienceAssetAttributesInfo](#amlmerklescienceassetattributesinfo)* | MerkleScience attributes |         |
| `createdAt`     | `created_at`     | *DateTime*                                                                                  | Create date time         |         |
| `updatedAt`     | `updated_at`     | *DateTime*                                                                                  | Update date time         |         |

### AmlChainalysisAssetAttributesInfo

| REST name                    | gRPC name                       | type                                                                         | description                                     | example |
|------------------------------|---------------------------------|------------------------------------------------------------------------------|-------------------------------------------------|---------|
| `symbol`                     | `symbol`                        | *string*                                                                     | Chainalysis asset symbol                        | BTC     |
| `isCheckWithdrawalsEnabled`  | `is_check_withdrawals_enabled`  | *bool*                                                                       | Indicates that withdrawals check enabled        | true    |
| `isCheckDepositsEnabled`     | `is_check_deposits_enabled`     | *bool*                                                                       | Indicates that deposits check enabled           | true    |
| `isNotifyWithdrawalsEnabled` | `is_notify_withdrawals_enabled` | *bool*                                                                       | Indicates that notify withdrawals check enabled | true    |
| `transferFormat`             | `transfer_format`               | *[ChainalysisTransferFormat](#assets-models-chainalysistransferformat-enum)* | Update date time                                |         |

### ChainalysisTransferFormat (enum)

+ `HashAndIndex` - Hash and index
+ `HashAndAddress` - Hash and address
+ `Hash` - Hash

### AmlMerkleScienceAssetAttributesInfo

| REST name    | gRPC name     | type     | description                        | example |
|--------------|---------------|----------|------------------------------------|---------|
| `symbol`     | `symbol`      | *string* | MerkleScience asset symbol         | BTC     |
| `currency`   | `currency`    | *int*    | MerkleScience currency ID          | 12      |
| `isDisabled` | `is_disabled` | *bool*   | Indicates that asset check enabled | true    |

### BrokerageAssetAttributesCreateInfo

| REST name             | gRPC name               | type      | description               | example |
|-----------------------|-------------------------|-----------|---------------------------|---------|
| `minDepositThreshold` | `min_deposit_threshold` | *decimal* | Minimum deposit threshold | 0.0001  |
| `minWithdrawalAmount` | `min_withdrawal_amount` | *decimal* | Minimum withdrawal amount | 0.1     |

### BrokerageAssetAttributesInfo

| REST name             | gRPC name               | type       | description               | example |
|-----------------------|-------------------------|------------|---------------------------|---------|
| `id`                  | `id`                    | *long*     | ID of the info            | 300001  |
| `minDepositThreshold` | `min_deposit_threshold` | *decimal*  | Minimum deposit threshold | 0.0001  |
| `minWithdrawalAmount` | `min_withdrawal_amount` | *decimal*  | Minimum withdrawal amount | 0.1     |
| `createdAt`           | `created_at`            | *DateTime* | Create date time          |         |
| `updatedAt`           | `updated_at`            | *DateTime* | Update date time          |         |
