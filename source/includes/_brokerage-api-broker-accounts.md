# Broker accounts

Broker account is the multi-currency (multi-blockchain) account which can contain unlimited number of personalized accounts within it. Broker account aggregates funds deposited to the personalized accounts and it provides funds for the withdrawals. The private keys of the broker accounts are stored in the vault associated with the broker account. The vault can't be changed once broker account is created.

You can choose which particular blockchain should be enabled for the given broker account. You can give a name to the broker account which make sense for you. It's convenient to have separate broker account for each business case. It's also reasonable to keep funds on the different brokerage accounts because of security reasons. For instance you can have a "Hot" broker account for operational work, "Cold" broker account to keep reserve funds which can be used by the limited employees group of your company, and "Frozen" broker account which can be used only by the owners of the business. Then you can setup a process to settle funds between these broker accounts on a periodical basis or on a demand.

Under the hood a broker account manages set of blockchain addresses. It has at least one address for each blockchain enabled for it. Depending on a particularities of each blockchain separate address or unique destination tag is created for each account within the broker account. Deposits received on an address of an account automatically moved to the address of the broker account. For some assets it's needed to pay fees in a different asset to move funds from an account to the broker account. For this Sirius provides "fees assets" on the account address by moving them from the broker account address.

Broker account structure:

<img src="https://github.com/swisschain/Sirius.Api.Docs/raw/master/source/images/broker-account-structure.png" alt="Broker account structure lifecycle"/>

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