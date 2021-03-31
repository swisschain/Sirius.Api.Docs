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

## VaultType (enum)

Type of the vault

+ `private` - The vault executing on the customer's environment
+ `shared` - The shared vault executing on the SwissChain environment used by different customers

## DepositState (enum)

State of the deposit

+ `detected` -
+ `confirmed` -
+ `completed` -
+ `failed` -
+ `cancelled` -
+ `provisioned` -
+ `unknownAsset` - 
+ `amlValidation` -
+ `amlFailed` -
+ `amlReviewed` -
+ `amlFailureAccepting` -
+ `refunding` -
+ `refundingProvisioned` -
+ `refunded` -

## DepositType (enum)

Type of the deposit

+ `tiny` - tiny deposit in native asset
+ `broker` - deposit on the broker account details
+ `regular` - deposit on account details in native token (ETH, BTC)
+ `token` - deposit on account details in custom token (ERC20)
+ `tinyToken` - deposit on account details in custom token (ERC20)

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
