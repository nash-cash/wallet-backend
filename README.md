
#### Master Build Status
[![Build Status](https://travis-ci.org/nashcash/nashcash-wallet-backend-js.svg?branch=master)](https://travis-ci.org/nashcash/nashcash-wallet-backend-js)

#### NPM
[![NPM](https://nodei.co/npm/nashcash-wallet-backend.png?compact=true)](https://npmjs.org/package/nashcash-wallet-backend)

#### Github

https://github.com/Nash-Cash/wallet-backend-js

# nashcash-wallet-backend

Provides an interface to the nashcash network, allowing wallet applications to be built.

* Downloads blocks from the network, either through a traditional daemon, or a blockchain cache for increased speed
* Processes blocks, decrypting transactions that belong to the user
* Sends and receives transactions

## Installation

NPM:

`npm install nashcash-wallet-backend --save`

Yarn:

`yarn add nashcash-wallet-backend`

## Documentation

You can see a list of all the other classes on the right side of the screen.
Note that you will need to prefix them all with `WB.` to access them, if you are not using typescript style imports, assuming you imported with `const WB = require('nashcash-wallet-backend')`.


### Javascript

```javascript
const WB = require('nashcash-wallet-backend');

(async () => {
    const daemon = new WB.Daemon('127.0.0.1', 24888);
    /* OR
    const daemon = new WB.Daemon('api.nashcash.net', 443);
    */
    
    const wallet = WB.WalletBackend.createWallet(daemon);

    console.log('Created wallet');

    await wallet.start();

    console.log('Started wallet');

    wallet.saveWalletToFile('mywallet.wallet', 'hunter2');

    /* Make sure to call stop to let the node process exit */
    wallet.stop();
})().catch(err => {
    console.log('Caught promise rejection: ' + err);
});
```

### Typescript

```typescript
import { WalletBackend, Daemon, IDaemon } from 'nashcash-wallet-backend';

(async () => {
    const daemon: IDaemon = new Daemon('127.0.0.1', 24888);

    /* OR
    const daemon: IDaemon = new Daemon('api.nashcash.net', 443);
    */

    const wallet: WalletBackend = WalletBackend.createWallet(daemon);

    console.log('Created wallet');

    await wallet.start();

    console.log('Started wallet');

    wallet.saveWalletToFile('mywallet.wallet', 'hunter2');

    /* Make sure to call stop to let the node process exit */
    wallet.stop();
})().catch(err => {
    console.log('Caught promise rejection: ' + err);
});
```

## Configuration

There are a few features which you may wish to configure that are worth mentioning.

### Auto Optimize

Auto optimization is enabled by default. This makes the wallet automatically send fusion transactions when needed to keep the wallet permanently optimized.

To enable/disable this feature, use the following code:

```javascript
wallet.enableAutoOptimization(false); // disables auto optimization
```

### Coinbase Transaction Scanning

By default, coinbase transactions are not scanned.
This is due to the majority of people not having solo mined any blocks.

If you wish to enable coinbase transaction scanning, run this line of code:

```javascript
wallet.scanCoinbaseTransactions(true)
```

### Logging

By default, the logger is disabled. You can enable it like so:

```javascript
wallet.setLogLevel(WB.LogLevel.DEBUG);
```

and in typescript:

```typescript
wallet.setLogLevel(LogLevel.DEBUG);
```

The logger uses console.log, i.e. it outputs to stdout.

If you want to change this, or want more control over what messages are logged,
you can provide a callback for the logger to call.

```javascript
wallet.setLoggerCallback((prettyMessage, message, level, categories) => {
    if (categories.includes(WB.LogCategory.SYNC)) {
        console.log(prettyMessage);
    }
});
```

and in typescript:

```typescript
wallet.setLoggerCallback((prettyMessage, message, level, categories) => {
    if (categories.includes(LogCategory.SYNC)) {
        console.log(prettyMessage);
    }
});
```


*

### Building (For Developers)

`git clone https://github.com/nashcash/nashcash-wallet-backend-js.git`

`cd nashcash-wallet-backend`

`npm install -g yarn` (Skip this if you already have yarn installed)

`yarn build`

Generated javascript files will be written to the dist/lib/ folder.

### Running tests

`yarn test` - This will run the basic tests

`yarn test-all` - This will run all tests, including performance tests.

### Before making a PR

* Ensure you are editing the TypeScript code, and not the JavaScript code (You should be in the `lib/` folder)
* Ensure you have built the JavaScript code from the TypeScript code: `yarn build`
* Ensure you have updated the documentation if necessary - Documentation is generated from inline comments, jsdoc style.
* Ensure you have rebuilt the documentation, if you have changed it: `yarn docs`
* Ensure the tests all still pass: `yarn test`, or `yarn test-all` if you have a local daemon running.
* Ensure your code adheres to the style requirements: `yarn style`

You can try running `yarn style --fix` to automatically fix issues.
