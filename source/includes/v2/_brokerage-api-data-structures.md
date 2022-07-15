# Data structures

## TagType (enum)

Tag type of the account. Tag is additional information associated with particular account, 
which can be specified in the blockchain transaction. This allows to use single blockchain 
address to handle many accounts on it. This feature is available only on some blockchains: 
(Ripple, Stellar, Eos...). In some blockchains different types of the tag are supported,
and you have to specify exact type in this case.

+ `text` - text tag
+ `number` - numeric tag

## WithdrawalDocument (object)

+ `properties` (*map<string,string>*) set of key-value pairs containing custom or *[known properties](_known-properties.md)*
+ `assetId` (*number*) - ID of the asset to use
+ `brokerAccountId` (*number*) - ID of the broker account to use
+ `destinationDetails` (*[DestinationDetails](#destinationdetails-object)*) - Destination details
+ `amount` (*decimal*) - Withdrawal amount

## DestinationDetails (object)

+ `address` (*string*) withdrawal destination address
+ `destinationTag` (*[DestinationTag](#destinationtag-object)*) - destination tag value and type

## DestinationTag (object)

+ `value` (*string* *optional*) string value of the tag
+ `tagType` (*[TagType](#tagtype-enum)*) - type of the tag to use
