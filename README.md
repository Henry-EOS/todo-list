# dApp todo-list

## Introduction

Based on the sample code of https://github.com/FeeSimple/dapp-skeleton, some needed modification
was made so that the existing code can work with new EOSIO software version.

It demonstrates CRUD operation of data against EOS blochchain via smart contract

### Required versions

* **EOSIO:** `1.0.6`
* **eosjs:** `^15.0.0`

## Javascript code snippet of eosjs library usage for contract/blockchain interaction

```
// Import eosjs
import EOS from 'eosjs'

// Define the account used to deploy the contract.
// This account will be used to reference the contract.
const contractAccount = {
    name: 'todolistapp1',
    privKey: '5KLpeUz1oYWojwJp4rSE5FhTPobfZr66x68FduLPwfv3iYcKFeP'
}

// Define the local nodeos endpoint connected to the remote testnet blockchain
const testnetNode = 'http://jungle.cryptolions.io:18888'

// Basic configuration of the EOS client
const config = {
    chainId: '038f4b0fc8ff18a4f0842a8f0564611f6e96e8535901dd45e43ac8691a1c4dca',
    keyProvider: contractAccount.privKey,
    httpEndpoint: testnetNode,

    expireInSeconds: 60,
    broadcast: true,
    debug: false, // set to true for debugging the transaction
    sign: true
}

// Instantiate the EOS client used for blockchain/contract interaction
const eosClient = EOS(config)

// Reference the deployed contract (via the account name used to deploy the contract)
// to invokve its method
eosClient.contract(contractAccount.name).then((contract) => {
  contract.create(
    contractAccount.name,
    (id),
    description,
    { authorization: [contractAccount.name] }
  ).then((res) => { this.setState({ loading: false })})
  .catch((err) => { this.setState({ loading: false }); console.log(err) })
})

```

## Source code explanation

### Part 1: frontend

Implemented in ReactJS that provides a web-based frontend with contract interaction.
Via the web UI, it's possible to create new "todo" item or delete the existing "todo" item.

### Part 2: contract

Implement the smart contract functions invoked by the frontend

## Usage

### Deploy contract

General instruction of how to deploy contract can be found right here:
https://github.com/FeeSimple/deploy_contract

Command line for deployment is as follows:

```
cleos set contract todolistapp1 path_to_dapp_contract_folder path_to_dapp_contract_folder/todo.wast path_to_dapp_contract_folder/todo.abi
```

* Install with: `npm install`
* Start up with: `npm run start`
* Open `http://localhost:8080/`
* Query on-chain database table: `cleos get table todolistapp1 todolistapp1 todos`
