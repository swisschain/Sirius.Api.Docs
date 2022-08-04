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

### Complex parameter values

Complex parameter values such as hashmaps or collections are represented as a single parameter encased with single quotes,
in which several chunks of data may appear separated by a semicolon (`;`). In the example below there are two parameters, 
first one representing a collection of values and second one is a hashmap:

`['1.2;34.0;123.1;12.0','keyOne:valueOne;keyTwo:valueTwo']`

### Special characters

Since original parameter values may contain special characters, used to encase values or denote boundaries between chunks 
of data, their values have to be escaped with a backslash (`\`). The comprehensive list of special characters is defined below:
- `:` separates key and its corresponding value in hashmaps, escaped as `\:`
- `;` separates key-value pairs in hashmaps and values in collections, escaped as `\;`
- `'` encases parameter values, escaped as `\'`
- `\` used to escape special characters, escaped as `\\`

In the example below there are several parameters with special characters in their original values:

`['Ocean\'s eleven','keyOne:value\:One;key\;Two:valueTwo','\\path\\to\\directory\\targetFile.txt']`

### Empty and unset values

Some parameters are optional, but since their position in the signing data format is rigid, it is not possible to simply omit them.
Instead, the absence of its value has to be denoted explicitly with `null` without quotes.
In the example below there are two parameters, the first one with value and the second one with value unset:

`['Parameter Value One',null]`

The absence of a complex parameter value (i.e. when a hashmap parameter is unset) should be treated in the same way.

### Numeric values

Parameters with decimal (or BigDecimal) values with or without fractional part have to be formatted with at least one fractional digit. Usually such parameters represent amounts of assets.
Thus, for example 2 has to become 2.0 in the signing data:
`['2.0']`

### Configurable properties

Most API V2 requests support assigning custom properties in the form of set of key-value pairs. Since 
assigning properties can change the way request will be handled on the serverside (see *[known properties](_known-properties.md)*),
properties are included in the signing data. Properties normally appear at the very end of signing data format
as a single parameter, encased in single quotes (`'`). Properties data is concatenated without any whitespace characters
with colon (`:`) as a separator between a key and its corresponding value, and semicolon (`;`) as a separator between 
key-value pairs. In the signing data format order of properties is important. It is required to sort them alphabetically 
by keys. 

In the example below there are two regular parameters, followed by four custom properties, sorted by keys alphabetically:

`['parameter Value One','124662357832','BrokerageExternalId:445566778899;UserId:12345;UserValidatorId:dr3413;WalletName:TestWallet']`

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

If the properties are not present at all, it is required to denote their absence with `null` similar to any other parameter.
