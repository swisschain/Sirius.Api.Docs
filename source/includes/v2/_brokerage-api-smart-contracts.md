# SmartContracts
<Placeholder_for_description>

## Invoke smart contract method

### Request

**Rest API:** `POST /api/v2/smart-contracts/{smartContractId}/invoke`
**gRPC API:** `Swisschain.Sirius.Api.ApiContract.V2.SmartContracts.Invoke`


```json
POST /api/v2/smart-contracts/{smartContractId}/invoke

> Request: (application/json, text/plain, */*)

x-idempotency-id: 1a5c0b3d15494ec8a390fd3b22d757d6

{
  "Document": {
    "Properties": {
      "prop1": "value1",
      "prop2": "value2"
  },
  "MethodName": "mint",
  "MethodAddress": "40c10f19",
  "BrokerAccountId": 100000074,
  "ReferenceId": "1a5c0b3d15494ec8a390fd3b22d757d6",
  "Arguments": [
    {
      "ParameterName": "account",
      "Value": {
        "Prototype": "address",
        "Address": "0x94afa005a395e470EB56a26E9081dC650EF84732"
      }
    },
    {
      "ParameterName": "amount",
      "Value": {
        "Prototype": "int",
        "Int": 100000000000000000
      }
    }
  ],
  "Payment": {
    "AssetId": 300000010,
    "Amount": 0
  }
  },
  "Name": "MyContractInvocation",
  "Signature": "JiOtcENJqblnWFcTZ4DiXwUZudlKBUtYnSkUjmfAH/8a1jeMPwFBu8MGjffITh3PRl/VLRTj8kjKWFT8EUR/8Qb3E8dTeuBlCIDU2RlHRRfb6iuTvjlPmUYQ/+Es6oZlH+Tapvjfa57N19poNZ7zimqadsASe3FGt0ME9J5vufE="
}
```


```protobuf
message SmartContractInvokeRequest {
  string idempotency_id = 1;
  SmartContractInvocationDocument document = 2;
  bytes signature = 3;
  int64 smart_contract_id = 4;
  string name = 5;
}

message SmartContractInvocationDocument {
  int64 broker_account_id = 1;
  string method_name = 2;
  google.protobuf.StringValue method_address = 3;
  repeated swisschain.sirius.api.smart_connect.data_model.Argument arguments = 4;
  swisschain.sirius.api.smart_contracts.common.Payment payment = 5;
  google.protobuf.StringValue reference_id = 6;
  map<string,string> properties = 7;
}


// example:
{
  "idempotency_id": "60f0cea8-1836-4ff5-b81d-422c1f705bce",
  "document": {
    "broker_account_id": 100000074,
    "method_name": "mint",
    "method_address": "40c10f19",
    "arguments": [
      {
        "parameterName": "account",
        "value": {
          "prototype": "address",
          "address": "0x94afa005a395e470EB56a26E9081dC650EF84732"
        }
      },
      {
        "parameterName": "amount",
        "value": {
          "prototype": "int",
          "int": 100000000000000000
        }
      }
    ],
    "payment": {
      "assetId": 300000010,
      "amount": 0
    },
    "reference_id": "1234",
    "properties": {
      "exampleKey": "exampleValue"
    }
  },
  "signature": "JiOtcENJqblnWFcTZ4DiXwUZudlKBUtYnSkUjmfAH/8a1jeMPwFBu8MGjffITh3PRl/VLRTj8kjKWFT8EUR/8Qb3E8dTeuBlCIDU2RlHRRfb6iuTvjlPmUYQ/+Es6oZlH+Tapvjfa57N19poNZ7zimqadsASe3FGt0ME9J5vufE=",
  "smart_contract_id": 109000043,
  "name": "ExampleName"
}
```

### Query Parameters

| gRPC name            | REST name                     | Type                                                                                       | Description                                                                               | Location |
|----------------------|-------------------------------|--------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|----------|
| `idempotency_id`     | `idempotencyId`               | string                                                                                     | Unique ID for idempotency                                                                | header   |
| `smart_contract_id`  | `smartContractId`             | int64                                                                                      | ID of the deployed smart contract                                                        | path     |
| `document.broker_account_id` | `document.brokerAccountId` | int64                                                                                      | Broker account ID                                                                        | body     |
| `document.method_name` | `document.methodName`       | string                                                                                     | Method name to be invoked                                                                | body     |
| `document.method_address` | `document.methodAddress` | string                                                                                     | Method address to be invoked                                                             | body     |
| `document.arguments` | `document.arguments`          | *repeated*, *[Argument](#argument)*                                                        | Arguments to the method invocation                                                       | body     |
| `document.payment`   | `document.payment`            | *[Payment](#payment)*                                                                      | Payment for the smart contract                                                           | body     |
| `document.reference_id` | `document.referenceId`     | string                                                                                     | Reference ID                                                                             | body     |
| `document.properties` | `document.properties`        | map<string, string>                                                                        | Properties of the smart contract                                                         | body     |
| `signature`          | `signature`                   | bytes                                                                                      | Base64-encoded RSA signature of the document signed with the Customer's private key    | body     |
| `name`               | `name`                        | string                                                                                     | Name                                                                                     | body     |

### Response


```json
{
  "SmartContractInvocationId": 123456789
}
```

```protobuf
message SmartContractInvokeResponse {
  oneof kind {
    SmartContractInvokePayload payload = 0;
    SmartContractInvokeError error = 1;
  }
}

message SmartContractInvokePayload {
  int63 id = 1;
}

message SmartContractInvokeError {
  enum ErrorCode {
    UNKNOWN = -1;
    INVALID_PARAMETERS = 0;
    DOMAIN_PROBLEM = 1;
    TECHNICAL_PROBLEM = 2;
    NOT_AUTHORIZED = 3;
    NOT_ENOUGH_BALANCE = 4;
  }

  ErrorCode code = 0;
  string message = 1;
}
```

### Signing data format

Smart Contract Invocation V2 signing data format (order of parameters is important):

```['422895a404f220d08763','createForgeBond','0x6ff569ba','100000077',null,'basicTokenInput:{callable:false;couponFrequencyInMonths:4;currency:GBP;denomination:1000;divisor:1;firstCouponDate:1693898940;initialMaturityDate:1693898940;initialSupply:10000;interestRateInBips:4;isSoftBullet:false;isinCode:EDOL12347788;name:EDOL12347788;owner:0x4750c623eB306c39d8312EBcE8ab3FDC12260b0a;registrar:0x4750c623eB306c39d8312EBcE8ab3FDC12260b0a;settler:0x4750c623eB306c39d8312EBcE8ab3FDC12260b0a;softBulletPeriodInMonths:0;startDate:1686209340;symbol:EDOL12347788};registryAddress:0x99b7a5170d2F89bB22a295DE41B548eB4A345bc0','300638515','0.0','CastOperationType:Issuance;FactoryAddress:0xAd3df4b196d2fbafecd6CFa6914B73d2d5812da3;FactoryName:BondBSC;InstrumentType:Bond;OwnerAddress:0x4750c623eB306c39d8312EBcE8ab3FDC12260b0a;OwnerBrokerAccountId:100000077;RegistrarAddress:0x4750c623eB306c39d8312EBcE8ab3FDC12260b0a;RegistrarBrokerAccountId:100000077;SettlerAddress:0x4750c623eB306c39d8312EBcE8ab3FDC12260b0a;SettlerBrokerAccountId:100000077']```


Parameters used:

0: IdempotencyId `422895a404f220d08763`

1: MethodName `createForgeBond`

2: MethodAddress `0x6ff569ba`

3: BrokerAccountId `100000077`

4: ReferenceId `null`

5: Arguments `basicTokenInput:{...};registryAddress:0x99b7a5170d2F89bB22a295DE41B548eB4A345bc0`

6: AssetId `300638515`

7: PaymentAmount `0.0`

8: Properties `CastOperationType:Issuance;FactoryAddress:0xAd3df4b196d2fbafecd6CFa6914B73d2d5812da3;...`

When constructing the signing data, ensure the properties are sorted alphabetically by keys. Despite gRPC and REST APIs accepting properties as a hashmap of unordered key-value pairs, these pairs will be internally sorted during signature validation on the server-side.

This signing data format is used whenever a client wishes to execute a signed Smart Contract Invocation V2. The signing data must be created on the client side and signed with the customer's private key.

Notes on the signing data format:

- Please use `null` without quotes for parameters without value (including properties)
- Please format numeric values according to their type. Numeric data types that can contain a fractional part (e.g., decimal, double, float, etc.) must be formatted with at least one decimal digit. Numeric data types that cannot contain a fractional part (e.g., integer, long, short) must be formatted without a fractional part and without a decimal separator.

For more information on the signing data format and its usage, please refer to the page dedicated to *[signing data formats](_signing-data.md)*.
