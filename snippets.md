# Gnoll Code Snippets

The following code snippets are just blocks of code I found or created that I wanted to remember. This page is primarily used as a reference for me but if you find any code useful then I am glad. :)

### Cleos Command to Call Contract Function

You have to unlock wallet first.

```
cleos -u API_ENDPOINT push action ACCOUNT_NAME upsert "[\"PARAM_1\",\"PARAM_2\"]" -p ACCOUNT_NAME@active
```

### Vanilla JS to Pull Table Rows from DAPP Network

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

### Chess Board Array [['a1','a2'...],['b1','b2',...]...]

```
// multi-dim array
[...Array(8)].map((y, i) => [...'abcdefgh'].map(x => x + (8 - i)))

// single array
[...Array(8)].map((y, i) => [...'abcdefgh'].map(x => x + (8 - i))).flat()
```
