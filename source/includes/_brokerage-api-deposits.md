# Deposits

Deposits reflects incoming transfers of the funds either to an account or directly to the broker account. You can get deposits history, or be notified about new deposits. There are different options to be notified about deposits being happened:
* You can pull deposit updates REST API
* You can listen for deposit updates gRPC stream
* You can register a web-hook get the notifications (coming soon)

Every deposit associated a broker account, an asset, optionally an account, it has amount, fees and state. If the account associated with the deposit has reference ID, you can find this value in the deposit too. 

In the case if the deposit amount is
too small to transfer funds from the account address to the broker account address because amount even doesn't cover fees or not significantly greater than fees, the deposited funds will be keept on the account address until the next deposit with enough amount. We use term "Tiny" for such deposits. If deposit destination is a broker account instead of an account, we name such deposits "Broker deposits".

The lifecycle of a deposit:

<img src="https://github.com/swisschain/Sirius.Api.Docs/blob/master/source/images/deposit-lyfecycle.png" alt="Deposit lifecycle"/>

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