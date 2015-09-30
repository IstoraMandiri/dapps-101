# Ethereum Dapps 101

This guide will take you through the nature, usage and deployment process of a 'hello world' ethereum dapp.

Before you get started, please kick off the following installation process in the background so it's ready by the time we need to use it:

### Installation Instructions

### OSX

First we need homebrew.

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Then install etheruem tools:

```
brew tap ethereum/ethereum;
brew update;
brew install ethereum;
brew install cpp-ethereum;
brew linkapps cpp-ethereum;
```

If you have alredy installed `geth` use `reinstall` in place of `install`


### Ubuntu

```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum-qt
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
sudo apt-get install cpp-ethereum
```

---

The [Wallet Dapp](https://github.com/ethereum/meteor-dapp-wallet) is also recommended to try out.

Optionally, you can also install [Embark](https://github.com/iurimatias/embark-framework/wiki/Installation) and [Meteor](http://github.com/meteor/meteor), which I'll be using at the end but is not required for a 'hello world' deploy.

## Ethereum in 10 seconds

A globally distributed virtual machine with a virtually perfectly reliable outcome.

Basically, it's a technology that lets us create apps with super powers.

## What is a Dapp?

❗️ **User Beware - Still in Alpha - [Here be dragons](https://en.wikipedia.org/wiki/Here_be_dragons)**

Dapps are apps that run on top of an ethereum blockchain. They make use of *smart contracts* and typically have a frontend UI for allowing users to interact with a particular set of smart contracts.

They can do all the things that normal apps can do, plus:

* **Ultra Reliable** No central server to go down; only goes down if you go down
* **Trustless** In theory it's the same way you trust '1 + 1 = 2'
* **Uncensorable** Legal questions like ip, privacy, censorship, updating, etc.
* **Cost** There is an intrinsic tie to the efficiency of the code and the cost of running the contract, so having well-designed contracts is super important in the long run. Optimising the code of very popular or specialized smart contracts will surely be a profitable profession in the near future.

> **[Current Gas Prices](http://ether.fund/tool/gas-fees)**
> Deploy original wallet: 1,230,162 gas = 12.3 ether = 1.44 USD  
> Deploy wallet stub: 184,280 = 1.8 ether = 0.21 USD  
> Simple Wallet transaction: 64,280 = 0.64 ether = 0.08 USD  

## An official dApp - Mist Wallet

* [Mist Browser](https://github.com/ethereum/mist)   
* [Wallet Dapp](https://github.com/ethereum/meteor-dapp-wallet) (standalone and/or in mist browser)
* Install: [https://blog.ethereum.org/2015/09/16/ethereum-wallet-developer-preview/](https://blog.ethereum.org/2015/09/16/ethereum-wallet-developer-preview/)   

Mist Wallet Technology:

* **Meteor** Full-Stack Javascript Framework
* **Electron** Chromium + Node.js Native App Wrapper
* **Solidity** [Smart Contract](https://github.com/ethereum/meteor-dapp-wallet/blob/master/wallet.sol)

## Components of a Dapp

* A. Collection of **Smart Contracts** (which eventually get deployed)
* B. Some kind of **User Interface** for interacting with the contracts
* C. **Connection** to a Node, usually localhost

### A. Smart Contracts

* Smart Contracts vs Dapps
* Solidity or Serpent (or LLL or Mutan)
* Each contract deployment has it’s own persistent state
* Modularity and structure can come from deploying separate contracts
* Data can be stored
   * `set` and `get`
   * Transaction (tx) data
   * In the future, SWARM / IPFS

#### Solidity Featues

**Objects**

* Contracts
* Structs (custom types)
* Functions
* Library (contract less context)
* Inheritence

**Types**

* Bools
* Usual Operators
* Bytes
* Ints
* Strings
* Arrays
* Addresses
* Implicit Conversions

**Concepts**

* Mortals
* Suicides
* Ownership
* [Kill](https://github.com/ethereum/wiki/wiki/Solidity-Tutorial#contract-inheritance)
* Balance
* Automatic `get` creation

**Exmaples**

* [SimpleStorage.sol](https://github.com/iurimatias/embark-framework/blob/develop/demo/app/contracts/simple_storage.sol)
* [FixedFeeRegistrar.sol](https://github.com/ethereum/dapp-bin/blob/master/registrar/FixedFeeRegistrar.sol)
* [Crowdfund.se](https://github.com/ethereum/serpent/blob/master/examples/crowdfund.se)

### B. User Interface

It's up to you:
      
* **Bareback** Just write a readme file that shows how people can interact via command line -- only the hardcore are likely to do this
* **Web** Cool for all the standard reasons, plus
   * `web3.js`
   * Client-only deploy (sereverless `file://`)
   * Deployable via [IPFS](https://ipfs.io/)
   * Mist Compatible
* **Native** Sure, why not?
   * Self-contained 'hosting'
* **Web + Native** Electron - best of both worlds? 

### C. Connection to Ethereum 

Install a client

* [eth](https://github.com/ethereum/cpp-ethereum) - c++  `brew install ethereum-cpp`
* [geth](https://github.com/ethereum/go-ethereum) - Go `brew install ethereum`


Let’s take a look at `geth` and it's basic config options:

```bash
geth

--networkid [random id or 1 for mainnet]
--genesis genesis_block.json

--datadir ~/Desktop/testnet # use a different one for each env
--logfile ~/Desktop/testnet.log

# so the mist wallet works
--ipcpath /Users/chris/Library/Ethereum/geth.ipc

--mine
--minerthreads 1

--password myPassword.txt
--unlock 9d4485d66959481f77c82b3bea26b197cc17d5e6
```

More info: https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options

#### Genesis

* [Dev Genesis Block](https://raw.githubusercontent.com/iurimatias/embark-framework/develop/demo/config/genesis/dev_genesis.json) - just 0000...
* [MainNet Genesis Block](http://jev.io/genesis_block.json) - now included in geth?

#### Network ID

* 1 = MainNet
* 0 = Public TestNet
* n = Private TestNet

Experimenting on the MainNet is too costly, so we we use a testnet on a random networkId during development, which allows us to deploy contracts for free by mining every block.

## Set up your account on a testnet

If this is your first time you'll want to create an account with a password that will hold your testnet ether, and can be used to make transactions with contracts.

```
# createa a data dir
mkdir ~/Desktop/testnet;

# start up the node
geth --networkid 1337 --genesis dev_genesis.json --datadir ~/Desktop/testnet  --ipcpath /Users/chris/Library/Ethereum/geth.ipc;

# lets set up or account
$ geth attach

# now you're in the geth REPL
> personal.listAccounts
> web3.eth.coinbase

# create a new account
> personal.newAccount("testPassword")

# take note of the accouont name
# set and check the coinbase
> personal.listAccounts
> web3.miner.setEtherbase(web3.eth.accounts[0])
> web3.eth.coinbase

# now we can start mining
> miner.start(1)
> web3.fromWei(eth.getBalance(web3.eth.coinbase), "ether")
> miner.stop()

# to deploy the wallet dapp we need to unlock our acccount
> personal.unlockAccount(web3.eth.coinbase)
```

*Demo: Official Wallet Dapp*

#### FYI - Connecting to a real network

The `genesis_block.json` is used, it contians pre-mine information and the state of the first block.

```
geth --genesis ~/git/ether/genesis_block.json --datadir ~/Desktop/mainnet
```

At the moment this can take a real long time, so let's just stick with the testnet for now!

## Web3.js

An important piece of the puzzle because JS is everywhere; web3.js creates a javascript interface for easily interacting with contracts in Javascript. Now we can interact with smart contract directly from a web interface, including from within a native electron app or on the www, etc.

https://github.com/ethereum/web3.js/tree/master

*Demo: Prettify the ABI file in browser-solidity*

Extra params to allow web apps to use your geth client:

```
--rpc
--rpcapi eth,web3
--rpcport 8101
--rpcaddr localhost
--rpccorsdomain *
```

❗️**Danger: make sure you configure `rpccorsdomain` properly.** 

## Let's deploy a 'hello world' Dapp

### Step 0. Pick your IDE

* Online editors eg. http://meteor-dapp-cosmo.meteor.com/
* Mix https://github.com/ethereum/wiki/wiki/Mix:-The-DApp-IDE
* Use a text editor (e.g. atom, sublime text + syntax plugins)
* Figure out a compile step (deploy via cosmo?)

### Step 1. Prepare Your Project

Take a look at `/dapp-1`

```
contracts/
  SimpleChat.sol
client/
  index.html
```

### Step 2. Compile + Deploy your contracts

https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#compiling-a-contract

Make sure `geth` is running on a testnet with rcp enabled.

```
geth --networkid 1337 --genesis dev_genesis.json --datadir ~/Desktop/testnet --ipcpath /Users/chris/Library/Ethereum/geth.ipc --rpc --rpcapi eth,web3 --rpcport 8545 --rpcaddr localhost --rpccorsdomain "*";
```

Open the geth console and start a miner

```javascript
geth attach;
miner.start(3); // 3 CPUs
```

Paste in the contract source code (solidity)

```javascript
source = "contract SimpleStorage { uint public storedData; function SimpleStorage(uint initialValue) { storedData = initialValue; } function set(uint x) { storedData = x; } function get() constant returns (uint retVal) { return storedData; } }";
```

Unlock your account

```javascript
personal.unlockAccount(eth.accounts[0]);
```

Compile it in `geth`, deploy by sending a transaction to it

```javascript
contract = eth.compile.solidity(source).SimpleStorage;
SimpleStorageContract = eth.contract(contract.info.abiDefinition);
SimpleStorage = SimpleStorageContract.new(42, {from: eth.accounts[0], data:contract.code, gas: 2000000}, function(e,c){
  if(c.address){
    console.log("Mined Contract", c.address);
  }
});
```

Wait for the message. If you don't see one make sure you're mining.

```javascript
SimpleStorage.set.sendTransaction(3, {from: eth.accounts[0]})
```

Note that you can set `eth.defaultAccount = eth.accounts[0]` to automatically set the `from` field, so you can use:

```javascript
SimpleStorage.set(5);
```

Done!

### Step 3. Deploy your UI

Add web3.js to project (index.html)

```html
<!-- index.html -->
<script>
  web3.setProvider(new web3.providers.HttpProvider('http://localhost:8545'));
</script>
```

Play with chrome console

```javascript
// in chrome
web3.fromWei(web3.eth.getBalance(web3.eth.coinbase), "ether").toNumber()
```

Set your default account

```javascript
// in chrome
web3.eth.defaultAccount = web3.eth.accounts[0];
```

Add the contracts ABIs. Get the ABI from `geth`:

```javascript
// inside `geth` client
> JSON.stringify(contract.info.abiDefinition)
```

Paste result into the chrome console, replace contract address with actual contract address.

```javascript
// inside chrome
myAbi = JSON.parse("[{\"constant\":true,\"inputs\":[],\"name\":\"storedData\",\"outputs\":[{\"name\":\"\",\"type\":\"uint256\"}],\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"name\":\"x\",\"type\":\"uint256\"}],\"name\":\"set\",\"outputs\":[],\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"get\",\"outputs\":[{\"name\":\"retVal\",\"type\":\"uint256\"}],\"type\":\"function\"},{\"inputs\":[{\"name\":\"initialValue\",\"type\":\"uint256\"}],\"type\":\"constructor\"}]");

SimpleStorageContract = web3.eth.contract(myAbi);

SimpleStorage = SimpleStorageContract.at("0xc05e8b3549857274024a96e38e7aa2e9499f2c6d");
```

Note: You can, also deploy contracts with `new`, just like in `geth`.

Play with the console; let's get and set the contract with just deployed.

```
SimpleStorage.get();
SimpleStorage.set(2);
SimpleStorage.get();
```

Cool, it works! Transfer the above code into the .html file.

See the transactions happening in `geth`.

Now let's make it interactive!

```
Make the buttons work...
```

TADA! You now have your first DAPP

## Frameworks

Now we've deployed a contract, we know how to do it manually - but if it is not automated it needlessly take up our time. Enter the frameworks...

* Roll your own workflow (with grunt, gulp)
* Web IDE only
* Truffle
* Embark
* Eris

*Embark* assumes you're deploying a web app. Let's see what embark does.

* Automatically sets up accounts
* `embark blockchain` instead of massive command string
* `embark deploy` instead of manual deploying (it redeploys all contracts at once)
* Can be configured for auto file watching
* Compiles Sol/Serpent into Web3.js ABIs
* Includes a 'Smart' Mining Script

Embark `embark blockhain` command:

```bash
geth --datadir /tmp/embark --logfile /tmp/embark.log --port 30303 --rpc --rpcport 8101 --rpcaddr localhost --networkid 10252 --rpccorsdomain * --minerthreads 1 --mine --genesis config/genesis.json --rpcapi eth,web3 --maxpeers 4 --password config/password --unlock xxx
```

### Meteor Embark

As a Meter dev, I took it a step further with `meteor-embark`, which integrates embark transparently into meteor, so it'll automatically manage blockchains and deployments whenever I run `meteor`.

**Cool thing:** Server-side geth client; semi-decentralized mobile deployment?!

### Noteworthy Tools

* AlethZero
* Mix

### A Closer look at Dapps

* The State of DApps [http://dapps.ethercasts.com/](http://dapps.ethercasts.com/)
* https://github.com/ethereum/dapp-bin [gavofyork]
* https://github.com/ethereum/serpent/tree/master/examples
* https://github.com/drupalnomad/ethereum-contracts
* https://github.com/androlo/EthereumContracts

#### Alarm Clock

TODO: Explain why it's interesting

[https://github.com/pipermerriam/ethereum-alarm-clock](https://github.com/pipermerriam/ethereum-alarm-clock)

contract CallerPool  
getMinimumBond, depositBond, withdrawBond, awardMissedBlockBonus, enterPool, exitPool, ...

contract Alarm  
deposit, withdraw, getNextBlockWithCall, getNextCallKey, getNextCallSibling, getCallLeftChild, getCallRightChild, placeCallInTree, rotateTree, unauthorizedAddress, authorizedAddress, addAuthorization, removeAuthorization, checkAuthorization, getLastCallKey, getCallContractAddress, getCallScheduledBy, getCallCalledAtBlock, getCallGracePeriod, getCallTargetBlock, getCallBaseGasPrice, getCallGasPrice, getCallGasUsed, getCallABISignature, checkIfCalled, checkIfSuccess, checkIfCancelled, getCallDataHash, getCallPayout, getCallFee, getCallData, registerData, getCallerPoolAddress, doCall, getCallMaxCost, getCallFeeScalar, getCallKey, scheduleCall, cancelCall, ...


### Dapp Design / Best Practices

* Automatic UI?
* Instead of writing `getLastN()`, just use `get(n)`, and implement the former on the clientside
this means you write less to the block-chain resulting in a cheaper dapp. (is this even true?)
* Think about smart contracts like microservices that can be used potentially by many other dapps
* Cucumber?
* Deploying a new versions?

A new contract has a new address, and somehow you need to design around that.

Eris' DOUG architecture; structures of smart contracts that implement inter-contract naming and permissions


# Further REading

* https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions
* https://eng.erisindustries.com/tutorials/2015/03/11/solidity-1/

---

https://github.com/ethereum/homebrew-ethereum/issues/38