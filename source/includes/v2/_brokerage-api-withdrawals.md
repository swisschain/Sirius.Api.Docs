# Withdrawals

Withdrawal refers to outgoing transfers of the funds from a broker account. You can get withdrawals history, or be notified about new withdrawals and state changing of the existing withdrawals. There are different options to be notified about withdrawals state changing:

* You can pull withdrawals updates REST API
* You can listen for withdrawal updates gRPC stream
* You can register a web-hook to get the notifications

Withdrawal can be initiated by you via API or Universe portal UI. Every withdrawal is associated with a broker account,
an asset, optionally an account, your internal ID, it has amount, fees and state. You can provide "WalletId" property with 
the value of account reference ID in order to associate an account to the withdrawal. This link
doesn't affect funds flow, it's used just for the reporting and notifying you back with withdrawal updates. Also you can 
provide "UserId" property with the value of UserNativeId in the users dictionary. This association is optional and can 
be used to enable AML verification procedure.

The fees required for the withdrawal transaction are paid atop of the specified amount.

The lifecycle of a withdrawal:

<img src="https://raw.githubusercontent.com/swisschain/Sirius.Api.Docs/master/source/images/withdrawal-lifecycle.png" alt="Withdrawal lifecycle"/>

## Starts a withdrawal

### Request

**Rest API:** `POST /api/v2/withdrawals`
**gRPC API:** `swisschain.sirius.api.v2.withdrawals.Execute`


```json
POST /api/v2/withdrawals

> Request: (application/json)

X-Idempotency-ID: 36585f34-d311-433a-87f1-1751b08480c3
X-TFA-code: 785441

{
  "document":{
    "properties":{
      "WalletId":"12345",
      "UserId":"test"
    },
    "assetId":300578305,
    "brokerAccountId":100000140,
    "destinationDetails":{
      "address":"0x663e933ECdc5b1acbaCB87F4aa1636cd05837613"
    },
    "amount":0.1
  },
  "signature":"pqG0st53GVkdBHIqCHsg9cB4noO1nyXlM8UGkZ+bC0fmvCbuf0W78GYj8ZuCtPMWl0XDIh5hVjwsjIPhiUfCl5KVUIA8La3EunRJZ5UkW95IvjcPCn+z5atzb8JQxPee8q2ehaMY+UNH08p1VmIjqJE1eKFzzs8ZxITu0gtwbZ4="
}
```

```protobuf
swisschain.sirius.api.v2.withdrawals.Execute

> Requests: (application/grpc)

message WithdrawalExecuteRequest {
  string idempotency_id = 1; // '36585f34-d311-433a-87f1-1751b08480c3'
  WithdrawalDocument document = 2;
  bytes signature = 3; // a6 a1 b4 b2 de 77 19 59 1d 04 72 2a 08 7b 20 f5 c0 78 9e 83 b5 9f 25 e5 33 c5 06 91 9f 9b 0b 47 e6 bc 26 ee 7f 45 bb f0 66 23 f1 9b 82 b4 f3 16 97 45 c3 22 1e 61 56 3c 2c 8c 83 e1 89 47 c2 97 92 95 50 80 3c 2d ad c4 ba 74 49 67 95 24 5b de 48 be 37 0f 0a 7f b3 e5 ab 73 6f c2 50 c4 f7 9e f2 ad 9e 85 a3 18 f9 43 47 d3 ca 75 56 62 23 a8 91 35 78 a1 73 ce cf 19 c4 84 ee d2 0b 70 6d 9e
}

message WithdrawalDocument {
  map<string,string> properties = 1; // { "WalletId":"12345", "UserId":"test" }
  int64 asset_id = 2; // 300578305
  int64 broker_account_id = 3; // 100000140
  DestinationDetails destination_details = 4;
  swisschain.sirius.api.common.BigDecimal amount = 5; // 0.1
}

message DestinationDetails {
  string address = 1; // "0x663e933ECdc5b1acbaCB87F4aa1636cd05837613"
  DestinationTag destination_tag = 2; // null
}

message DestinationTag {
  google.protobuf.StringValue value = 2;
  swisschain.sirius.api.common.TagType type = 3;
}

```

### Parameters

REST name | gRPC name                    | type                                               | REST placement | description 
--------- |------------------------------|----------------------------------------------------| -------------- | -----------
`X-Idempotency-ID` | -                            | *string*                                           | *header* | Unique ID of the request
`X-TFA-code` | -                            | *string*                                           | *header* | TFA code
 - | `idempotency_id`             | *string*                                           | - | Unique ID of the request
`document` | `document`                   | *[WithdrawalDocument](#withdrawaldocument-object)* | *body* | structured withdrawal parameters
`document.properties` | `document.properties`        | *map<string, string>*                              | *body* | dictionary of key-value pairs with custom or *[known properties](_known-properties.md)*
`document.assetid` | `document.asset_id`          | *int64*                                            | *body* | ID of the asset to use
`document.brokerAccountId` | `document.broker_account_id` | *int64*                                            | *body* | ID of the broker account to use
`document.destinationDetails` | `document.destination_details` | *[DestinationDetails](#destinationdetails-object)* | *body* | destination parameters of the withdrawal (address and tag info)
`document.Amount` | `document.amount` | *[DestinationDetails](#destinationdetails-object)* | *body* | destination parameters of the withdrawal (address and tag info)
`signature` | `signature`                  | *optional*, *string*                               | *body* | Base64-encoded RSA signature of the data signed with the Customer's private key. The exact format of the data to be signed is described below.

Withdrawal document format (order of parameters is important):
```
['36585f34-d311-433a-87f1-1751b08480c3','300578305','100000140','0x663e933ECdc5b1acbaCB87F4aa1636cd05837613','null','null','0.1','UserId:test','WalletId:12345']
```

Parameters used:
0: IdempotencyId `36585f34-d311-433a-87f1-1751b08480c3`
1: AssetId `300578305` (BNB)
2: BrokerAccountId `100000140`
3: DestinationAddress `0x663e933ECdc5b1acbaCB87F4aa1636cd05837613`
4: DestinationTag `null`
5: DestinationTagType `null`
6: Amount: `0.1`
7: Properties:
7.1: UserId: `test` (corresponds to UserNativeId)
7.2: WalletId: `12345` (corresponds to AccountReferenceId)

Order of Properties sent from the client side is not important. However, it is important to sort them alphabetically by keys during document creation. Received pairs of properties will be sorted during signature validation internally.
 
The presented document format is used whenever client wishes to execute signed withdrawal. Withdrawal document has to be constructed on the client side and signed with customer private key.

Notes on the document format:
- Please use string literal `null` for parameters without value
- Please format numeric values with at least one fractional digit, even if the value is an integer (i.e. 2 has to become 2.0)

### Query Parameters

### Response

```json
POST /api/v2/withdrawals

> Response: 200 (application/json) - success response

{
 "id":106000003
}
```

```protobuf
swisschain.sirius.api.withdrawals.Withdrawals.Execute

> Response: (application/grpc) - success response

message WithdrawalExecutePayload {
  int64 id = 1;
}

message WithdrawalExecuteError {
  enum WithdrawalExecutionErrorCode {
    UNKNOWN = 0;
    INVALID_PARAMETERS = 1;
    DOMAIN_PROBLEM = 2;
    TECHNICAL_PROBLEMS = 3;
    NOT_AUTHORIZED = 4;
    NOT_ENOUGH_BALANCE = 5;
  }

  WithdrawalExecutionErrorCode code = 1;
  string message = 2;
}

message WithdrawalExecuteResponse {
  oneof result {
    WithdrawalExecutePayload payload = 1;
    WithdrawalExecuteError error = 2;
  }
}
```
### Parameters

REST name | gRPC name                    | type                                               | REST placement | description
--------- |------------------------------|----------------------------------------------------| -------------- | -----------
`id ` | `id ` | *number* | ID of the withdrawal
