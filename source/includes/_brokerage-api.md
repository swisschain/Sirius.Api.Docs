# Brokerage API

## Search blockchains

We distinguish ***Blockchain***, ***Protocol***, and ***Network Type***.

***Blockchain*** is not just Bitcoin or Ethereum, it's a particular network. For example Bitcoin MainNet, Ethereum Ropsten are blockchains, whilst Bitcoin and Ethereum are protocols which are used by these blockchains. Networks can be one of the types - `private`, `test`, `public`. So Blockchain has a protocol and network type.

***Blockchain*** is an "instance" of the protocol - Ethereum Ropsten, Bitcoin Mainnet, Ethereum Mainnet.

Protocol is a set of rules on how blockchain nodes interact with each other - `Bitcoin`, `Ethereum`, `Ripple`.

Network type can be `private`, `test`, or `public`.

### Request

**gRPC:** `sirius.Brokerage.GetBlockchains`

**Rest API:** `GET /api/blockchains`

### Query Parameters

name | description
---- | -----------
`id` | Exact ID of the blockchain to search
`name` | Text to search in the name of the blockchain
`protocolCode` | Exact ode of the protocol to search
`protocolName` | Text to search in the name of the protocol
`networkType` | List of the network types to search
`order` | Result items sorting order
`cursor` | Cursor to continue the search
`limit` | Maximum number of items to return in the search results

### Response

Array of blockchain descriptions:

name | type | description | example
---- | ---- | ----------- | -------
`id` | string | Unique identifier. | `bitcoin-mainnet`
`name` | string |Name of the blockchain. | `Bitcoin MainNet`
`networkType` | [NetworkType](#network-type) | Type of the network
`protocol` | [BlockchainProtocol]() | Protocol of the blockchain.


```json
GET /api/blockchains

> Response 200 (application/json) - success response

{
  "pagination": {
    "cursor": "bitcoin-mainnet",
    "count": 2,
    "order": "asc",
    "nextUrl": "/api/blockchains?order=asc&cursor=ripple-mainnet&limit=2"
  },
  "items": [
    {
      "name": "Litecoin - MainNet",
      "id": "litecoin-mainnet",
      "networkType": "public",
      "protocol": {
        "code": "litecoin",
        "name": "Litecoin"
      }
    },
    {
      "name": "Ripple - MainNet",
      "id": "ripple-mainnet",
      "networkType": "public",
      "protocol": {
        "code": "ripple",
        "name": "Ripple"
      }
    },
  ]
}
```

```protobuf

# todo:

```


