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

export default class Blockchain {
  constructor () {
    this.accessContext = false
    this.walletProviders = false
    this.selectedProvider = false
    this.wallet = false
    this.locked = false

    this.name = false
    this.authorization = false
  }

  setup (network) {
    let config = false
    switch (network) {
      case 'eos':
        config = {
          appName: 'EOSMainDfuse',
          network: {
            blockchain: 'eos',
            chainId: 'aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906',
            host: 'mainnet.eos.dfuse.io/',
            port: 443,
            protocol: 'https'
          },
          walletProviders: [
            scatter()
          ]
        }
        break
      case 'jungle':
        config = {
          appName: 'EOSJungleDfuse',
          network: {
            blockchain: 'eos',
            chainId: 'e70aaab8997e1dfce58fbfac80cbbb8fecec7b99cf982a9444273cbc64c41473',
            host: 'jungle.eos.dfuse.io/',
            port: 443,
            protocol: 'https'
          },
          walletProviders: [
            scatter()
          ]
        }
        break
      case 'telos':
        config = {
          appName: 'TelosFoundation',
          network: {
            blockchain: 'eos',
            chainId: 'cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f',
            host: 'api.telosfoundation.io',
            port: 443,
            protocol: 'https'
          },
          walletProviders: [
            scatter()
          ]
        }
        break
      default:
        config = {
          appName: 'LocalNodeos',
          network: {
            blockchain: 'eos',
            chainId: 'cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f',
            host: 'localhost',
            port: 8888,
            protocol: 'http'
          },
          walletProviders: [
            scatter()
          ]
        }
    }
    this.accessContext = initAccessContext(config)
    this.walletProviders = this.accessContext.getWalletProviders()
  }

  selectWallet (index) {
    this.selectedProvider = this.walletProviders[index]
    this.wallet = this.accessContext.initWallet(this.selectedProvider)
  }

  connect (success = () => {}, failure = () => {}) {
    let main = this
    if (main.locked) return

    async function connectWallet () {
      main.locked = true
      await main.wallet.connect()
    }

    connectWallet().then((res) => {
      main.locked = false
      success(res)
    }).catch(e => {
      main.locked = false
      failure(e)
    })
  }

  async login (success = () => {}, failure = () => {}, notConnected = () => {}) {
    let main = this
    if (main.wallet.connected === true) {
      let discoveryData = await main.wallet.discover({ pathIndexList: [ 0, 1, 2, 3 ] })
      if (discoveryData.keyToAccountMap.length > 0) {
        // await wallet.login(accountName, authorization)
        alert('You have more than one account. Insert code.')
      } else {
        try {
          await main.wallet.login()
          success()
        } catch (e) {
          notConnected(e)
        }
      }
    } else {
      notConnected()
    }
  }

  async logout (cb = () => {}) {
    await this.wallet.terminate()
    cb()
  }
}
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

## C++ : Time Since X Validation

```
// Original author @theblockstalk
// Modified for convenience

TABLE data {
  name    user;
  time_point last_updated;
  auto primary_key() const { return user.value; }
};
typedef multi_index<name("data"), data> data_table;

ACTION theclass::thefunction(name from) {
  data_table _data(get_self(), get_self().value);

  // remove record if added within 1 hour
  auto data_itr = _data.find(from.value);
  if (data_itr != _data.end()) {
    const time_point now = current_time_point();
    const time_point last_updated = data_itr->last_updated;
    const uint32_t elapsed_time_sec = now.sec_since_epoch() - last_updated.sec_since_epoch();
    check(elapsed_time_sec < (60 * 60), "Record was not added within 1 hour");
    data_itr = _data.erase(data_itr);
  }
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
