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

### Argument parameter values

The arguments in the signing data represent inputs to the smart contract method being invoked. Similar to 
complex parameters, smart contract argument parameter values are represented as a single parameter encased 
with single quotes. Each Argument object consists of a parameter name and its corresponding value, and these 
are separated by a colon (`:`).

The Argument objects are sorted alphabetically by their parameter names. They are then appended to 
the signing data format string, with each Argument object separated by a semicolon (`;`).

Different types of arguments are formatted as follows:

**Void**: A void argument type doesn't have a value to be represented, so the value should not appear in the signing data. Example:
```'voidParameterName:'```


**Bool**: A boolean argument is represented as either `true` or `false`. Example:
```'flag:true'```

**Bytes**: Byte array arguments are Base64-encoded. Example:
```'byteArray:SGVsbG8='```

This is for the byte array equivalent of the string "Hello".

**Decimal**: A BigDecimal argument is formatted with at least one fractional digit, even if the value is an integer. Example:
```'decimalValue:2.0'```

**Int**: A BigInteger argument is represented as is. Example:
```'intValue:123'```

**String**: String arguments are formatted in the same way as string parameters, i.e. with special characters escaped. Example:
```'stringValue:Hello\:world'```

**Address**: Ethereum address arguments are represented as is. Example:
```'address:0x663e933ECdc5b1acbaCB87F4aa1636cd05837613'```


**Timestamp**: A timestamp argument is represented in the same format it was passed to the API originally, 
following RFC 3339 format used in `google.protobuf.Timestamp`, i.e. `{year}-{month}-{day}T{hour}:{min}:{sec}[.{frac_sec}]Z`. Example:
```'timestampParam:2017-01-15T01:30:15.01Z''```

**Enum**: An enumeration argument is represented as a string. Example:
```'enumValue:OPTION_ONE'```

**Array**: Array arguments are formatted as a string with elements separated by a semicolon (`;`). Example:
```'arrayArg:{value1;1234}'```

**Composite**: Composite arguments are objects that contain fields. These fields are sorted alphabetically 
by name and formatted as a string of key-value pairs. Example:
```'composite:{'field1':'value1','field2':'value2'}'```


**Map**: Map arguments are formatted as a string with entries separated by a semicolon. Each entry is formatted 
as a key-value pair with a colon separating the key from the value. Example:
```'arg1:{mapKeyB:mapValueB;mapKeyA:mapValueA}'```
NB: map arguments ARE NOT sorted alphabetically by the keys, since the keys can theoretically be of different type, including
complex ones. Since it is not feasible to compare for instance an array with a timestamp for the purpose of sorting them, 
it was decided to format map arguments in the order they were provided.


**Either**: An either argument can have one of two possible types. The value is formatted based on its actual type (as described above). The argument name indicates which type it is. Example:
```'arg1:{field1:value1}'```

If the list of Argument objects is empty or null, it should be represented as null without quotes.

The arguments' values have to be formatted correctly based on their types. For instance, if the value is a BigDecimal,
it is important to ensure that there's at least one fractional digit. If it's a String, the special characters have to be escaped.
In case of complex types like Array, Composite, or Either, the elements inside are sorted alphabetically 
by their keys before they are formatted for signing. However, the complex type of Map does not require sorting and it the 
signing data will be constructed in the original order of key-value pairs of map.

In the case where Argument objects contain complex parameter values like Array, Map, Composite, or Either, 
each value is formatted and encased in curly brackets {}. Each value inside these complex types is separated 
by a semicolon (;), and in the case of Map, Composite and Either, each key-value pair is separated by a colon (:).

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

Numeric parameters, specifically those with data types that can contain a fractional part (such as decimal, double, float, etc.),
must be formatted with at least one decimal digit, regardless of whether they have a fractional part or not.
These parameters often represent quantities, such as asset amounts.

For example, a decimal value of 2 should be represented as 2.0 in the signing data:

`['2.0']`

This rule ensures consistency in how numeric values are presented in the signing data.

On the other hand, numeric data types that do not support a fractional part (e.g., integer, long, short, etc.) 
must be formatted without a fractional part and without a decimal separator symbol. For instance, an integer value 
of 2 should remain as 2 in the signing data:

`['2']`

It's important to distinguish between these two cases to prevent any ambiguity when processing the signing data.

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
