# Smart Contract API

## Introduction

```shell
contract EthereumPOS {
    function subscriptionActive(int256 id) constant returns (bool);
    function subscriptionStartedDate(int256 id) constant returns (string);
    function subscriptionStartedBlock(int256 id) constant returns (int);
    function subscriptionExpiresDate(int256 id) constant returns (string);
    function subscriptionExpiresBlock(int256 id) constant returns (int);
    function subscriptionAddressActive(address wallet) constant returns (bool);
    function orderPaid(int256 id) constant returns (bool);
    function orderPaidBlock(int256 id) constant returns (int);
    function approvedSender() constant returns (address);
}
```

Ethereum POS includes a ethereum smart contract so your contracts can reference it run functionality 
within the blockchain within your own environment.

### Contract Calls

Value | Description
--------- | -----------
subscriptionActive | will response true or false if subscription is active/paid
subscriptionStartedDate | subscription starting date as a string in UTC format
subscriptionStartedBlock | subscription initial block as int
subscriptionExpiresDate | subscription expiration date as a string in UTC format
subscriptionExpiresBlock | subscription initial block as int
subscriptionAddressActive | subscription for a ref_address specified with new order
orderPaid | responds true or false if order was paid
orderPaidBlock | responds the block number when order was paid
approvedSender | returns the ETH address of the trusted Ethereum POS account


## Contract Callbacks

> Insert the Ethereum POS Contract API into your contract


```shell
function callback(int256 id, string ref_id, int256 amount, address from) {
    require(msg.sender == EthereumPOS.approvedSender());
    // run your functions here
    
}
```


When you choose to use Ethereum POS contract callback feature, you must include a callback 
function inside of your contract. Ethereum POS will submit a contract call to the callback address.



## Example Contract

```shell
pragma solidity ^0.4.11;

contract EthereumPOS {
    function subscriptionActive(int256 id) constant returns (bool);
    function subscriptionStartedDate(int256 id) constant returns (string);
    function subscriptionStartedBlock(int256 id) constant returns (int);
    function subscriptionExpiresDate(int256 id) constant returns (string);
    function subscriptionExpiresBlock(int256 id) constant returns (int);
    function subscriptionAddressActive(address wallet) constant returns (bool);
    function orderPaid(int256 id) constant returns (bool);
    function orderPaidBlock(int256 id) constant returns (int);
    function approvedSender() constant returns (address);
}


contract Example {

    EthereumPOS public pos;
    address public owner;
    
    mapping(int256 => bool) isPaid;
    mapping(address => bool) allowedUsers;

    function Example() {
        pos = EthereumPOS(0x0000000000000000000000000000000000000);
        owner = msg.sender;
    }
    
    // EtheruemPOS.com subscription callback request
    function callbackSubscription(uint256 id, string ref_id, address ref_address, bool active) {
        require(msg.sender == pos.approvedSender());
        allowedUsers[ref_address] = active;
        isPaid[ref_address] == active
    }
    
    function testSubscription() external payable {
       require(pos.subscriptionAddressActive(msg.sender));
       owner.transfer(msg.value);
    }
    
}
```

This is a full example of using Ethereum POS smart contract to run functions inside of another contract.

### Synopsis

Imagine you have your own smart contract that holds user subscriptions, each user that wants to 
interact with your contract must pay your monthly fee. 

In this example, this contract would allow ETH to be transferred to a different address if the sending address 
has a active subscription.


### testSubscription(address wallet) Function

This public function will force the sender to have a active subscription to continue. If the address has an 
active subscription the sending ETH amount will be sent to the owner.

