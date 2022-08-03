# V2 requests signing

V2 API supports signed requests with asymmetric cryptography. Since every type of request is structurally different, the
format of the data to be signed also varies. In order to sign a request, clientside has to put together a string 
in the specific format and sign that string with the customer's private key, thereby generating RSA signature. 
Every V2 API definition contains special-purpose optional field called "Signature", which has to be used to pass 
the RSA signature to the serverside.

The specific formats of the data to be signed are defined on the respective pages, describing an API endpoint. For clarity,
each API endpoint page contains subsection called "Signing data format" with detailed description of the parameters 
to be used for putting together textual data for signing.

For convenience of use, links to all formats' descriptions are compiled below:

- *[Withdrawal V2 execution signing data format](_brokerage-api-withdrawals.md#signing-data-format)*

## Signing format guidelines

While all requests serve various purposes and therefore consist of different parameters, they also follow several common
patterns. The way some of these patterns are used across V2 APIs in relation to the signing functionality is described below:

### General format description

Signing data is a sequence of bytes, corresponding to UTF-8-encoded string in the specific format. The beginning and ending 
of the signing data are denoted by square brackets: `[]`. Each parameter value is encased in single quotes: `''`.
Parameters are comma-separated without any whitespace characters before or after the separator.

In the example below there are three parameters, two textual ones and a single numeric parameter:  

`['parameter Value 1','parameter Value 2','26.7']`

### Empty and unset values

Some parameters are optional, but since their position in the signing data format is rigid, it is not possible to simply omit them.
Instead, the absence of its value has to be denoted explicitly with `null` in single quotes.
In the example below there are two textual parameters, the first one with value and the second one with value unset:

`['Parameter Value One', 'null']`

The only exception being custom properties. Since, if present, they always appear at the end of the signing data.
If not present, they can simply be omitted altogether.

### Numeric values

Parameters with integer values with or without fractional part have to be formatted with at least one fractional digit.
Thus, for example 2 has to become 2.0 in the signing data:
`['2.0']`

### Configurable properties

Most API V2 requests support assigning custom properties in the form of set of key-value pairs. Since 
assigning properties can change the way request will be handled on the serverside (see *[known properties](_known-properties.md)*),
properties are included in the signing data. Properties normally appear at the very end of signing data format,
each key-value pair denoted as a separate parameter, encased in single quotes. Key and value are concatenated 
together, separated by the semicolon without any whitespace characters before or after the separator. Order of properties 
is important only in the signing data. It is required to sort them alphabetically by keys.
In the example below there are two regular parameters, followed by four custom properties, sorted by keys alphabetically:

`['parameter Value One','124662357832','BrokerageExternalId:445566778899','UserId:12345','UserValidatorId:dr3413','WalletName:TestWallet']`

V2 APIs normally accept properties as a hashmap, with key-value pairs in arbitrary order. Received pairs of properties 
will be sorted during signature validation internally on the serverside. Following this logic, properties from the example above
can be sent to the serverside in the following way:
```json
{
  "properties": {
    "UserValidatorId": "dr3413",
    "WalletName": "TestWallet",
    "BrokerageExternalId": "445566778899",
    "UserId": 12345
  }
}
```

If the properties are not present at all, simply omit them from the signing data altogether.
