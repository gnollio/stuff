# Gnoll Code Snippets

The following code snippets are just blocks of code I found or created which I wanted to remember. This page is primarily used as a reference for me but if you find any of the code useful then I am glad. Have a good day. :)

## CLI : Cleos Command to Call Contract Function

You have to unlock wallet first.

```
cleos -u API_ENDPOINT push action ACCOUNT_NAME upsert "[\"PARAM_1\",\"PARAM_2\"]" -p ACCOUNT_NAME@active
```

## CLI & JS : Install EOSNY Transit

```
yarn add eosjs@beta eos-transit eos-transit-scatter-provider
```

```
import { initAccessContext } from 'eos-transit'
import scatter from 'eos-transit-scatter-provider'
...

const accessContext = initAccessContext({
  appName: 'my_first_dapp',
  network: {
    host: 'api.pennstation.eosnewyork.io',
    port: 7001,
    protocol: 'http',
    chainId: 'cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f'
  },
  walletProviders: [
    scatter()
  ]
})

const walletProviders = accessContext.getWalletProviders()

// grab first wallet available in list
const selectedProvider = walletProviders[0]

// select wallet and init
const wallet = accessContext.initWallet(selectedProvider)

// function to connect to wallet
async function connectToWallet () {
  await wallet.connect()
}

// wait for results
connectToWallet().then(() => {
  alert('Wallet found!')
 }).catch(e => {
  alert('Could not connect to a wallet.')
 })
```

## C++ : Action w/ Auto-Increment Key

```
ACTION classname::functionname(name from) {
  require_auth(from);

  // init table
  stuff_table _stuff(get_self(), get_self().value);

  // create wager record
  _stuff.emplace(from, [&](auto& row) {
    row.id = _stuff.available_primary_key();
  });
}
```

## Vanilla JS :  to Pull Table Row from DAPP Network

```
fetch('https://kylin-dsp-2.liquidapps.io/v1/dsp/ipfsservice1/get_table_row', {
  method: 'POST',
  mode: 'cors',
  cache: 'no-cache',
  credentials: 'same-origin',
  headers: {},
  redirect: 'follow',
  referrer: 'no-referrer',
  body: JSON.stringify({
    contract: 'CONTRACT_NAME',
    scope: 'CONTRACT_NAME',
    table: 'TABLE_NAME',
    key: 'PRIMARY_KEY_VALUE'
  })
}).then(response => { console.log(response.json()) })
```

## Vanilla JS : Chess Board Array [['a1','a2'...],['b1','b2',...]...]

```
// multi-dim array
[...Array(8)].map((y, i) => [...'abcdefgh'].map(x => x + (8 - i)))

// single array
[...Array(8)].map((y, i) => [...'abcdefgh'].map(x => x + (8 - i))).flat()
```
