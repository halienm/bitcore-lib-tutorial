### bitcore-lib-tutorial

This sameple code will let you generate a private key, create a transaction and submit it via with bitcore-lib and Javascript

---

code for the episode:

Creating a private key and sending a bitcoin transaction in 30 lines of code

`bitcore-lib.js` (purely didatical version - [this version])

`bitcoinjs-lib.js` (TODO: create a more practical version using bitcoinjs-lib or a wrapper)

```js
const { readFileSync } = require('fs')
const bitcoin = require('bitcore-lib')
const { PrivateKey, Transaction } = bitcoin

const pvtKeyString = readFileSync('./private-key.txt').toString().trim()
const pvtKey = new PrivateKey(pvtKeyString)

const address = pvtKey.toAddress().toString()

console.log("private key:", pvtKey.toString())
console.log("address:", address)

const getUTXOs  = require('./blockchain-api/get-utxos')
const pushTx    = require('./blockchain-api/push-tx')
// const { pushTx, getUTXOs } = require('./blockchain-api') // TODO: NPM MODULE

;(async () => {
  try {
    let utxos = await getUTXOs({ address })

    utxos = [ utxos[0] ]

    console.log("utxos:", utxos)

    const amount = 10000 // 10k sats (0.1mbtc)

    const tx = new Transaction()
      .from(utxos)
      .to(address, amount)
      .change(address)
      .fee(1000)
      .sign(pvtKey)

    const txHex = tx.serialize()

    console.log("tx:", txHex)

    const { success, txHash } = pushTx(txHex)
    console.log("success:", success, "txHash:", txHash)

  } catch (err) {
    console.error(err)
  }
})()

```

Creating a private key and sending a bitcoin transaction in 30 lines of code -  #coding #bitcoin #tx #utxo #bitcoin-tx #bitcoin-utxo


---

@makevoid
