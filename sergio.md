# Sergio Notes

Hi Sergio :) 

The following steps were taken to get Every EOS running on your server. Sorry if you already know most of this stuff, I wrote it for possibly other people who want to use the software and aren't as familiar with web software.

Note: This assumes a default Ubuntu server.

## Install Node Version Manager (NVM)

This software helps manage versions of Node.js software. Node is used by the Every EOS source code. Unlike PHP which is usually pretty solid along with Apache or Nginx, Node.js can be a pain to deal with sometimes, especially when it comes to different versions. NVM makes it easier to switch between versions without lots of configuration and conflicts.

#### Download and Install NVM
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```

#### Configure NVM
```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

```

#### Use NVM to Install Node.js

This command installs the latest version of Node.js.

```
nvm install node
```

## Create www Directory and Clone Every EOS

I'm just putting the files in /var/www/every-eos/, this isn't required, you can put them wherever you want.

```
mkdir /var/www
cd /var/www
git clone https://github.com/lky1001/every-eos.git
```

## Install Yarn and Reboot

Most Node.js apps use third-party libraries which are managed by a package manager such as NPM or Yarn. Here I am installing Yarn on this server.

#### Download and Install Yarn

```
curl -o- -L https://yarnpkg.com/install.sh | bash
```

#### Reboot Server

I had to reboot the server for yarn to be accessible at the terminal.

```
reboot
```

## Use Yarn to Install Application Dependencies (Third-Party Libraries)

Once you log back into your server after rebooting, go to the Every EOS directory and run the yarn command to download all the third-party libraries the application needs.

```
cd /var/www/every-eos/
yarn install
```

## Fix Git Errors

The code I cloned from GitHub had errors in two files, it looks like the cause was a bad git merge. These errors prevented me from launching the website. I corrected the errors doing the following below ...

Note: By the time you read this they may have already corrected these errors. Also, I am not familiar with the code so my corrections may have been incorrect in some parts, please check with author if issue occurs.

#### File: /var/www/every-eos/src/components/Wallet/MyTokens.js

It went from this ...

```
<<<<<<< HEAD
import React, { PureComponent, Fragment } from 'react'
=======
import React, { Component, Fragment } from 'react'
import { inject, observer } from 'mobx-react'
import { compose } from 'recompose'
>>>>>>> 1efa0e71f2b241536661611be77cfa9af5c30e66
import { ShadowedCard, MarketHeader } from '../Common/Common'
```

To this ...

```
import React, { PureComponent, Fragment } from 'react'
import { inject, observer } from 'mobx-react'
import { compose } from 'recompose'
import { ShadowedCard, MarketHeader } from '../Common/Common'
```

The bottom portion of the page had other merge issues starting at Line 211.

It went from this ...

```
                      <td style={{ fontSize: '15px' }}>{token.balance}</td>
<<<<<<< HEAD
                      <td style={{ fontSize: '15px' }}>
                        {frozenTokens.find(t => t.token_id === token.id)
                          ? frozenTokens.find(t => t.token_id === token.id).frozen_amount.toFixed(4)
                          : '0.0000'}
                      </td>
                      <td style={{ fontSize: '15px' }}>1.000</td>
=======
                      <td style={{ fontSize: '15px' }}>{token.frozen}</td>
                      <td style={{ fontSize: '15px' }}>-</td>
>>>>>>> 1efa0e71f2b241536661611be77cfa9af5c30e66
                      <td style={{ fontSize: '15px' }}>
```

To this ...

```
                      <td style={{ fontSize: '15px' }}>{token.balance}</td>
                      <td style={{ fontSize: '15px' }}>{token.frozen}</td>
                      <td style={{ fontSize: '15px' }}>-</td>
                      <td style={{ fontSize: '15px' }}>
```

#### File: /var/www/every-eos/src/stores/accountStore.js

Top of page went from this ...

```
<<<<<<< HEAD
import { decorate, observable, set, toJS, action, computed } from 'mobx'
import eosAgent from '../EosAgent'
import ApiServerAgent from '../ApiServerAgent'
import { loginUserMutation } from '../graphql/mutation/user'
import graphql from 'mobx-apollo'
import { frozenAmountTokensQuery } from '../graphql/query/order'
=======
import { decorate, observable, action } from 'mobx'
import graphql from 'mobx-apollo'
import eosAgent from '../EosAgent'
import ApiServerAgent from '../ApiServerAgent'
import { loginUserMutation } from '../graphql/mutation/user'
import { getFrozenBalance } from '../graphql/query/token'

>>>>>>> 1efa0e71f2b241536661611be77cfa9af5c30e66
class AccountStore {
```

To this ...

```
import { decorate, observable, set, toJS, action, computed } from 'mobx'
import eosAgent from '../EosAgent'
import ApiServerAgent from '../ApiServerAgent'
import { loginUserMutation } from '../graphql/mutation/user'
import graphql from 'mobx-apollo'
import { frozenAmountTokensQuery } from '../graphql/query/order'

class AccountStore {
```

Starting at Line 246 went from this ...

```
<<<<<<< HEAD
  getFrozenAmountTokens = async account_name => {
    this.frozenAmountTokens = await graphql({
      client: ApiServerAgent,
      query: frozenAmountTokensQuery,
      variables: { account_name }
    })
  }

  get frozenAmountTokensError() {
    return (this.frozenAmountTokens.error && this.frozenAmountTokens.error.message) || null
  }

  get frozenAmountTokensLoading() {
    return this.frozenAmountTokens.loading
  }

  get frozenAmountTokensList() {
    return (
      (this.frozenAmountTokens.data && toJS(this.frozenAmountTokens.data.frozenAmountTokens)) || []
    )
  }

  get frozenAmountTokensCount() {
    return this.frozenAmountTokens.data.frozenAmountTokens
      ? this.frozenAmountTokens.data.frozenAmountTokens.length
      : 0
  }
=======
  getFrozenBalance = async () => {
    return await graphql({
      client: ApiServerAgent,
      query: getFrozenBalance,
      variables: { account: this.loginAccountInfo.account_name }
    })
  }
>>>>>>> 1efa0e71f2b241536661611be77cfa9af5c30e66
}
```

To this (I removed bottom function) ...

```

  getFrozenAmountTokens = async account_name => {
    this.frozenAmountTokens = await graphql({
      client: ApiServerAgent,
      query: frozenAmountTokensQuery,
      variables: { account_name }
    })
  }

  get frozenAmountTokensError() {
    return (this.frozenAmountTokens.error && this.frozenAmountTokens.error.message) || null
  }

  get frozenAmountTokensLoading() {
    return this.frozenAmountTokens.loading
  }

  get frozenAmountTokensList() {
    return (
      (this.frozenAmountTokens.data && toJS(this.frozenAmountTokens.data.frozenAmountTokens)) || []
    )
  }

  get frozenAmountTokensCount() {
    return this.frozenAmountTokens.data.frozenAmountTokens
      ? this.frozenAmountTokens.data.frozenAmountTokens.length
      : 0
  }

}
```

Starting at Line 312 went from this ...

```
  subscribeLoginState: action,
<<<<<<< HEAD
  getFrozenAmountTokens: action
=======
  getFrozenBalance: action
>>>>>>> 1efa0e71f2b241536661611be77cfa9af5c30e66
})
```

To this ...

```
  subscribeLoginState: action,
  getFrozenAmountTokens: action
})
```

## Build Source Code

I tried to build the source code on the server but ran into a resource error, likely from there not being enough RAM on the server. To solve this I downloaded the code to my local computer and then ran the build command locally. I then uploaded the "build" folder to Every EOS directory as if it had been built on the server.

Build Folder Location: /var/www/every-eos/build/

The following command will build the source code. It will compile and put everything into a build folder at the path listed above. This build folder is the root folder you will point your web server to.

Run this command at the root of the every-eos directory (/var/www/every-eos/) to build the source.

```
npm run build
```

After it finishes, you should now see a build folder with all the compiled website files in it.

## Run Server

The Every EOS build process recommends using software called "serve" to act as a web server for testing.

#### Install Serve Software

```
npm install -g serve
```

It is now time to run the web server. I add a '&' at the end of the command to run this server as a process, this will allow me to logout of the server without stopping the web server. By default the web server software is for testing and doesn't run permanently unless you configure it to.

For production use you may want to install Apache or Nginx and point it to the build folder. I'm not an expert on web server software so might want to ask a more knowledgable person about this.

Anyways, here is the command to run the web server software. This will run the web server on the build folder. 

```
serve -s build
```

You should now see your site running at http://IP-ADDRESS:5000. To have it just work at the root of your IP Address or domain name you'll need to run the serve command like the following ...

```
serve -l 80 -s build
```

This will tell it to use port 80 which is the default web port that browser use.

That's it!
