# Known properties

V2 API allows assigning custom properties to some requests in the form
of set of key-value pairs where both key and value are strings. Users can provide custom
keys or use some known properties with predefined behaviour and functionality.

The list of the known properties and their respective behaviour is defined below:

1. `UserId`: allows passing identifier from the AML verification (formerly known as UserNativeID);
2. `WalletId`: allows linking request to specific account, the value is used to look up Account with the specified `AccountReferenceId`


