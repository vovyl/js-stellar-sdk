# Operations

## Overview

In order to read information about operations from a Horizon server, the [`server`](./server.md) object provides the `operations()` function. `operations()` returns an `TransactionCallBuilder` class, an extension of the [`CallBuilder`](./call_builder.md) class.

By default, `operations()` provides access to the [`operations_all`](https://github.com/stellar/horizon/blob/master/docs/reference/operations-all.md) Horizon endpoint.  By chaining other methods to it, you can reach other operation endpoints.

## Methods

| Method | Horizon Endpoint | Param Type | Description |
| --- | --- | --- | --- |
| `operations()` | [`operations_all`](https://github.com/stellar/horizon/blob/master/docs/reference/operations-all.md) | | Access all operations. | 
| `.operation("operationID")` | [`operations_single`](https://github.com/stellar/horizon/blob/master/docs/reference/operations-single.md) | `string` | Pass in the ID of a particular operation to access its details. |
| `.forAccount("address")` | [`operations_for_account`](https://github.com/stellar/horizon/blob/master/docs/reference/operations-for-account.md) | `string` | Pass in the address of a particular account to access its operations.|
| `.forLedger("ledgerSeq")` | [`operation_for_ledger`](https://github.com/stellar/horizon/blob/master/docs/reference/operation-for-ledger.md) | `string` | Pass in the sequence of a particular ledger to access its operations. |
| `.forTransaction("transactionHash")` | [`operations_for_transaction`](https://github.com/stellar/horizon/blob/master/docs/reference/operations-for-transaction.md) | `string` |  Pass in the hash of a particular transaction to access its operations. |
| `.limit(limit)` | | `integer` | Limits the number of returned resources to the given `limit`.|
| `.cursor("token")` | | `string` | Return only resources after the given paging token. |
| `.order({"asc" or "desc"})` | | `string` |  Order the returned collection in "asc" or "desc" order. |
| `.call()` | | | Triggers a HTTP Request to the Horizon server based on the builder's current configuration.  Returns a `Promise` that resolves to the server's response.  For more on `Promise`, see [these docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).|
| `.stream({options})` | | object of [properties](https://developer.mozilla.org/en-US/docs/Web/API/EventSource#Properties) | Creates an `EventSource` that listens for incoming messages from the server.  URL based on builder's current configuration.  For more on `EventSource`, see [these docs](https://developer.mozilla.org/en-US/docs/Web/API/EventSource). |


## Examples

```js
var StellarSdk = require('stellar-sdk');
var server = new StellarSdk.Server({hostname:'horizon-testnet.stellar.org', secure:true, port:443});

server.operations()
  .forAddress("GBS43BF24ENNS3KPACUZVKK2VYPOZVBQO2CISGZ777RYGOPYC2FT6S3K")
  .order("desc")
  .call()
  .then(function (operationResult) {
    console.log(operationResult);
  })
  .catch(function (err) {
    console.error(err);
  })
```