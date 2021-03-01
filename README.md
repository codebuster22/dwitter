# Web3 Tutorial
## Initialise Web3
Web3 require a provider through which it can connect to the ethereum network.
``` javascript
let web3 = new Web3(provider) 
```
metamask injects it's own web3, to access that, we can check
```js
if (typeof window.web3 !== undefined ){
    let web3js = new Web3(window.web3.getProvider());
}
```
or
```js
if (typeof window.web3 !== undefined ){
    return window.web3;
}
```
that is use web3 provided by metamask as it is. No need to reassign.

## Changing provider
we can use ```.setProvider()``` to change the currentProvider of web3.
```js
web3.setProvider(newProvider);
```
this will change the current provider to new provider.

## Given Provider
in some browser like mist, we get web3 from the browser as it is. we can get the provider and use that to connect to ethereum.
```js
let provider = web3.givenProvider;

// or

let providers = web3.providers

// or 

let provider = web3.currentProvider
```

### Note:- The address used here are checksum addresses that is osme lowecase and some uppercase. Based on this it calculates checksum to prove address correctness. Incorrect checksum throws error.

## Create new account

```js
let account = web3.eth.accounts.create([entropy]);

// Returns Object with private key and public key
```
Generates an account object with private key and public key.

### Parameters
1. entropy - String (optional): A random string to increase entropy. If given it should be at least 32 characters. If none is given a random string will be generated using randomhex.
### Returns
1. Object - The account object with the following structure:

    1. address - string: The account address.
    2. privateKey - string: The accounts private key. This should never be shared or stored unencrypted in localstorage! Also make sure to null the memory after usage.
    3. signTransaction(tx [, callback]) - Function: The function to sign transactions. See web3.eth.accounts.signTransaction() for more.
    4. sign(data) - Function: The function to sign transactions. See web3.eth.accounts.sign() for more.

## Instantiating new Contract

```js
let myContract = new web3.eth.Contract(jsonAbi[,address][,options];

// if no address given that is not deployed

let contractTrx = myContract.deploy(options);

// options - object
// have two properties
// data: contract bytecode
// arguments: [optional] if contract constructor takes any argument

// It returns an object with various properties, one is .sen()
// Will deploy the contract, promise type

contractTrx.send({
    from:,
    gas: ,
    gasPrice:,
    value: ,
})
.on('error', function(error){ ... })
.on('transactionHash', function(transactionHash){ ... })
.on('receipt', function(receipt){
   console.log(receipt.contractAddress) // contains the new contract address
})
.on('confirmation', function(confirmationNumber, receipt){ ... })
.then(function(newContractInstance){
    console.log(newContractInstance.options.address) // instance with the new contract address
});
```
returns instance of contract

## Methods
```js
myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
.on('transactionHash', function(hash){
    ...
})
.on('receipt', function(receipt){
    ...
})
.on('confirmation', function(confirmationNumber, receipt){
    ...
})
.on('error', console.error);
```

```.on()``` only available for transactions that is ```.send()``` and other functions similar to this. 
```.on()``` not available for ```.call()```

## Events

### Fire only once
```js
myContract.once(event[, options], callback)

// or

myContract.once('MyEvent', {
    filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
    fromBlock: 0
}, function(error, event){ console.log(event); });

// event output example
> {
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My String'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
    },
    event: 'MyEvent',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blockNumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
}
```

### Subscribing to events
```js
myContract.events.MyEvent([options][, callback])

// or

// Return type called EventEmitter
myContract.events.MyEvent((),function(error, event){ console.log(event); })
.on('data', function(event){
    console.log(event); // same results as the optional callback above
})
.on('changed', function(event){
    // remove event from local database
})
.on('error', console.error);

// event output example
> {
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My String'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
    },
    event: 'MyEvent',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blockNumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
}
```

This is fired everytime an event is emmited even if it concerns current user or not.

### Filtering through events

```js
// Let's say we have this event
event MyEvent(uint indexed myIndexedParam .....);
```

```js
myContract.events.MyEvent({
    filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
    fromBlock: 0
}, function(error, event){ console.log(event); })
.on('data', function(event){
    console.log(event); // same results as the optional callback above
})
.on('changed', function(event){
    // remove event from local database
})
.on('error', console.error);

// event output example
> {
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My String'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
    },
    event: 'MyEvent',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blockNumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
}

```