---
title: Ethereum POS API Reference

language_tabs:
  - shell: cURL
  - php
  - javascript: nodejs
  - go
  - python
  - ruby
  - c

toc_footers:
  - <a href='https://ethereumpos.com'>Sign Up for a API Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Ethereum POS API! This documentation covers everything you need to know about accepting ethereum for your application. You'll be able to create orders, retrieve order information, and get your account history.

Ethereum POS has a pretty basic transaction system.

1. Application creates new order for customer
2. Customer pays QR code Ethereum address
3. Successful alert for customer letting them know they paid. (expires in 15 minutes)
4. Application receives a POST to callback URL of order information and transaction ID.
5. Ethereum POS waits for 12 confirmations then sends entire wallet balance to your Ethereum address.
7. Another POST request to your application's callback URL.

We have language bindings in cURL, Golang, and in jQuery. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

Ethereum POS is currently in the process of making the transaction callback process a seamless and transparent process. The plan is to have the customer pay directly to Ethereum POS Contract with data to include your Ethereum address, order ID and callback information. Once you've collected your funds into your account inside of the contract, you'll be able to withdraw directly from contract, thus reducing the transaction fee each time a payment is transmitted.

### Production API Endpoint (Ethereum Blockchain ETH)
`https://api.ethereumpos.com`

### Testing API Endpoint (Ethereum Ropsten Testnet)
`https://testapi.ethereumpos.com`

# Authentication

Ethereum POS uses API keys to allow access to the API. You can register a new Ethereum POS API key at our [developer portal](http://example.com/register).

Ethereum POS expects for the API key to be included in all requests over $5.00 USD. API requests to the server in a header that looks like the following:

`Authorization: MYKEYHERE`

<aside class="notice">
You must replace <code>MYKEYHERE</code> with your personal API key.
</aside>

# Orders API

## Create New Order

```shell
curl -X POST -H "Authorization: MYAPIKEY" -H "Content-Type: application/json" -d '{
    "amount": "13.54",
    "callback": "http://domain.com"
}' "https://api.ethereumpos.com/order"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://api.ethereumpos.com/order');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'Authorization' => 'MYAPIKEY'
  'Content-Type' => 'application/json'
));

$request->setBody('{
       "amount": "13.54",
       "address": "0x3f05dc64b34f5063461f81d88972a3f542f57331",
       "callback": "http://domain.com"
   }');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```javascript
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.ethereumpos.com",
  "port": null,
  "path": "/order",
  "headers": {
    "Authorization": "MYAPIKEY",
    "Content-Type": "application/json"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
                "amount": "13.54",
                "address": "0x3f05dc64b34f5063461f81d88972a3f542f57331",
                "callback": "http://domain.com"
            });
req.end();
```

```go
NOT complete
```

```python
import http.client

conn = http.client.HTTPSConnection("api.ethereumpos.com")

payload = "{ \"amount\": \"13.54\", \"address\": \"0x3f05dc64b34f5063461f81d88972a3f542f57331\", \"callback\": \"http://domain.com\" }"

headers = { 'Authorization': "MYAPIKEY", 'Content-Type': "application/json" }

conn.request("POST", "/order", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.ethereumpos.com/order")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Authorization"] = 'MYAPIKEY'
request["Content-Type"] = 'application/json'
request.body = "{ \"amount\": \"13.54\", \"address\": \"0x3f05dc64b34f5063461f81d88972a3f542f57331\", \"callback\": \"http://domain.com\" }"

response = http.request(request)
puts response.read_body
```

```c
NOT complete
```

> The above command returns JSON structured like this:

```json
{
  "id": 716101402,
  "address": "0x1679548078de9a5a9129d75719b5e442abd5b2e5",
  "expected_amount": "0.4173",
  "currency": "usd",
  "currency_amount": "13.54",
  "payment_url": "https://ethereumpos.com/test_payment/716101402",
  "status": "pending",
  "eth_price": "69.98",
  "qr_code": "http://chart.apis.google.com/chart?cht=qr&chs=500x500&chl=ethereum%3A0x1679548078de9a5a9129d75719b5e442abd5b2e5&chld=H|0",
  "transaction_id": "",
  "sent_transaction": "",
  "created_at": "0001-01-01T00:00:00Z",
  "return_to": "0x888a5fEFAA912778FdBf8eC222F299e96057c43b",
  "paid": false,
  "expired": false
}
```

This endpoint creates NEW ORDERS for your customers to send money to.

### HTTP Request

`POST https://api.ethereumpos.com/order`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
amount | true | Currency value of order in USD ($25.85)
callback | false | Callback URL for your application for this order.
ref_id | false | Order ID based on your website or application (optional)
item_title | false | The item title to be shown to user (optional)
item_description | false | The item description for user (optional)
item_image | false | The item image to display to user (optional)


## Get Order Information

```shell
curl -X GET "https://api.ethereumpos.com/order/<ID>"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://api.ethereumpos.com/order/<ID>');
$request->setMethod(HTTP_METH_GET);

$request->setHeaders(array(
  'Authorization' => 'MYAPIKEY'
  'Content-Type' => 'application/json'
));

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```javascript
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.ethereumpos.com",
  "port": null,
  "path": "/order/<ID>",
  "headers": {
    "Authorization": "MYAPIKEY",
    "Content-Type": "application/json"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write();
req.end();
```

```go
NOT complete
```

```python
import http.client

conn = http.client.HTTPSConnection("api.ethereumpos.com")

headers = { 'Authorization': "MYAPIKEY", 'Content-Type': "application/json" }

conn.request("GET", "/order/<ID>", "", headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.ethereumpos.com/order/<ID>")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["Authorization"] = 'MYAPIKEY'
request["Content-Type"] = 'application/json'

response = http.request(request)
puts response.read_body
```

```c
NOT complete
```

> The above command returns JSON structured like this:

```json
{
  "id": 716101402,
  "address": "0x1679548078de9a5a9129d75719b5e442abd5b2e5",
  "expected_amount": "0.4173",
  "currency": "usd",
  "currency_amount": "29.20",
  "payment_url": "https://ethereumpos.com/test_payment/716101402",
  "status": "complete",
  "eth_price": "69.98",
  "qr_code": "http://chart.apis.google.com/chart?cht=qr&chs=500x500&chl=ethereum%3A0x1679548078de9a5a9129d75719b5e442abd5b2e5&chld=H|0",
  "transaction_id": "0x8586aebc2ae317388955f4cbf073aa736e8fe440aca7eec90f3bfe303e59abd6",
  "sent_transaction": "0x4746935c2b001f45d8c907e2bb425106411e03d8afc8940b47298a099509f48c",
  "created_at": "0001-01-01T00:00:00Z",
  "return_to": "0x888a5fEFAA912778FdBf8eC222F299e96057c43b",
  "paid": true,
  "expired": false
}
```

This endpoint retrieves order information

### HTTP Request

`GET https://api.ethereumpos.com/order/<ID>`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
ID | true | Ethereum POS Order ID


# Subscriptions API

## Create Subscription


```shell
curl -X POST -H "Authorization: MYAPIKEY" -d '{
    "amount": "13.54",
    "recurring": "1m",
    "callback": "http://domain.com"
}' "https://api.ethereumpos.com/subscription"
```


> The above command returns JSON structured like this:

```json
{
  "id": 32341402,
  "currency": "usd",
  "currency_amount": "29.20"
}
```

Create a Recurring Subscription for customers that are billed the same for each month/week/day/etc.

### HTTP Request

`POST https://api.ethereumpos.com/subscription`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
amount | true | Currency value of order in USD ($25.85)
callback | false | Callback URL for your application for this order.
recurring | true | This subscription is due every ___ days.

* recurring can be in format 14d, 1m, 2w format. (d=day, m=month, w=week)



## Create Subscription Order

This endpoint will create a new order for your subscription. 

```shell
curl -X POST -H "Authorization: MYAPIKEY" -d '{
    "id": 34654567,
    "ref_id": "2312324",
    "ref_address": "0x00000000000000000000000000000000",
    "callback": "http://domain.com"
}' "https://api.ethereumpos.com/subscription/order"
```

> The above command returns JSON structured like this:

```json
{
  "id": 736101402,
  "address": "0x1679548078de9a5a9129d75719b5e442abd5b2e5",
  "expected_amount": "0.4173",
  "currency": "usd",
  "currency_amount": "13.54",
  "payment_url": "https://ethereumpos.com/test_payment/716101402",
  "status": "pending",
  "eth_price": "69.98",
  "qr_code": "http://chart.apis.google.com/chart?cht=qr&chs=500x500&chl=ethereum%3A0x1679548078de9a5a9129d75719b5e442abd5b2e5&chld=H|0",
  "transaction_id": "",
  "sent_transaction": "",
  "created_at": "0001-01-01T00:00:00Z",
  "return_to": "0x888a5fEFAA912778FdBf8eC222F299e96057c43b",
  "paid": false,
  "expired": false,
  "subscription": 34654567
}
```



### HTTP Request

`POST https://api.ethereumpos.com/subscription/order`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
ID | true | Ethereum POS Subscription ID
callback | false | A callback URL to notify ETH payment
ref_id | false | Reference ID for your application
ref_address | false | Reference another ETH address for contract callbacks

```shell
curl -X GET "https://api.ethereumpos.com/subscription/<id>"
```





## Get Subscription

This endpoint retrieves subscription information

```shell
curl -X GET "https://api.ethereumpos.com/subscription/<ID>"
```

### HTTP Request

`GET https://api.ethereumpos.com/subscription/<ID>`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
ID | true | Ethereum POS Order ID




# History API

## Show All Orders

This endpoint retrieves all orders related to your account.

```shell
curl -X GET -H "Authorization: MYAPIKEY" "https://api.ethereumpos.com/history"
```


```php
<?php

$request = new HttpRequest();
$request->setUrl('https://api.ethereumpos.com/history');
$request->setMethod(HTTP_METH_GET);

$request->setHeaders(array(
  'Authorization' => 'MYAPIKEY'
  'Content-Type' => 'application/json'
));

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```javascript
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.ethereumpos.com",
  "port": null,
  "path": "/history",
  "headers": {
    "Authorization": "MYAPIKEY",
    "Content-Type": "application/json"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write();
req.end();
```

```go
NOT complete
```

```python
import http.client

conn = http.client.HTTPSConnection("api.ethereumpos.com")

headers = { 'Authorization': "MYAPIKEY", 'Content-Type': "application/json" }

conn.request("GET", "/history", "", headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.ethereumpos.com/history")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["Authorization"] = 'MYAPIKEY'
request["Content-Type"] = 'application/json'

response = http.request(request)
puts response.read_body
```

```c
NOT complete
```

> The above command returns JSON structured like this:

```json
{
  "orders": [
    {
      "id": 716101419,
      "address": "0x2524e3c29bd8ed8a16344ae0da27898dbef08511",
      "expected_amount": "0.2281",
      "currency": "usd",
      "currency_amount": "16.00",
      "payment_url": "",
      "status": "complete",
      "eth_price": "70.15",
      "qr_code": "",
      "transaction_id": "0x8403bcf744da5862eef5d3dd2fef951b06a582938e4848940bf0e87889d09012",
      "sent_transaction": "",
      "created_at": "0001-01-01T00:00:00Z",
      "return_to": "0x888a5fEFAA912778FdBf8eC222F299e96057c43b",
      "paid": true,
      "expired": false
    },
    ...
  ]
}
```

This endpoint retrieves order information

### HTTP Request

`GET https://api.ethereumpos.com/history`


## Show Orders with Status

```shell
curl -X GET -H "Authorization: MYAPIKEY" "https://api.ethereumpos.com/history/<METHOD>"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://api.ethereumpos.com/history/<METHOD>');
$request->setMethod(HTTP_METH_GET);

$request->setHeaders(array(
  'Authorization' => 'MYAPIKEY'
  'Content-Type' => 'application/json'
));

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```javascript
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.ethereumpos.com",
  "port": null,
  "path": "/history/<METHOD>",
  "headers": {
    "Authorization": "MYAPIKEY",
    "Content-Type": "application/json"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write();
req.end();
```

```go
NOT complete
```

```python
import http.client

conn = http.client.HTTPSConnection("api.ethereumpos.com")

headers = { 'Authorization': "MYAPIKEY", 'Content-Type': "application/json" }

conn.request("GET", "/history/<METHOD>", "", headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.ethereumpos.com/history/<METHOD>")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["Authorization"] = 'MYAPIKEY'
request["Content-Type"] = 'application/json'

response = http.request(request)
puts response.read_body
```

```c
NOT complete
```

> The above command returns JSON structured like this:

```json
{
  "orders": [
    {
      "id": 716101419,
      "address": "0x2524e3c29bd8ed8a16344ae0da27898dbef08511",
      "expected_amount": "0.2281",
      "currency": "usd",
      "currency_amount": "16.00",
      "payment_url": "",
      "status": "complete",
      "eth_price": "70.15",
      "qr_code": "",
      "transaction_id": "0x8403bcf744da5862eef5d3dd2fef951b06a582938e4848940bf0e87889d09012",
      "sent_transaction": "",
      "created_at": "0001-01-01T00:00:00Z",
      "return_to": "0x888a5fEFAA912778FdBf8eC222F299e96057c43b",
      "paid": true,
      "expired": false
    },
    ...
  ]
}
```

This endpoint retrieves order information

### HTTP Request

`GET https://api.ethereumpos.com/history/<METHOD>`

### URL Parameters

Parameter | Value
--------- | -----------
METHOD | pending,success,complete,expired,refunded

### Method Values

Method Value | Description
--------- | -----------
pending | Customer has not sent this transaction yet.
success | Customer has sent the correct amount and has at least 1 confirmation
complete | Ethereum POS sent the entire wallet balance to the creators Ethereum address
expired | Customer didn't send the transaction within 15 minutes. This can also happen if customer sent a low transaction fee.
refunded | Merchant has successfully refunded the customer


# System API

## Show Sever Status

```shell
curl -X GET "https://api.ethereumpos.com/status"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://api.ethereumpos.com/status');
$request->setMethod(HTTP_METH_GET);

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```javascript
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.ethereumpos.com",
  "port": null,
  "path": "/status"
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write();
req.end();
```

```go
NOT complete
```

```python
import http.client

conn = http.client.HTTPSConnection("api.ethereumpos.com")

conn.request("GET", "/status", "", "")

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.ethereumpos.com/status")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["Content-Type"] = 'application/json'

response = http.request(request)
puts response.read_body
```

```c
NOT complete
```

> The above command returns JSON structured like this:

```json
{
  "block": "966798",
  "test": true
}
```

This endpoint retrieves information about the merchant/developer account.

### HTTP Request

`GET https://api.ethereumpos.com/status`

### Response Values

Value | Description
--------- | -----------
block | the most up to date block with Ethereum and our servers.
test | will be set to true if request is sent to test API.



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
function callbackOrder(uint256 id, string ref_id, bool paid) {
    require(msg.sender == EthereumPOS.approvedSender());
    // run your functions here
    
}

function callbackSubscription(uint256 id, string ref_id, address ref_address, bool active) {
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



# Pricing and Fees

## Memberships

Ethereum POSing has simple the understand billing

### Monthly Plans

Plan | Price | Monthly Cost | Max (ETH) | Recurring Payments | Contract Calls
--------- | ------- | ------- | ------- | ----------- | --------
Basic     |    $0.25 per order    | $0.00   | 0.25 ETH |  yes  |  no
Seller    |    $0.15 per order    | $6.00   |  2 ETH   |  yes  |  no
Premium   |    $0.10 per order    | $25.00   |  10 ETH |  yes  | yes
Exclusive |    $0.00              | $50.00   |  100 ETH  |  yes  | yes
Corporate |    Contact Us             | ---  |  100+ ETH  |  yes  | yes

### Smart Contract Features

Cost Per Call | Gas Limit |
--------- | -----------
$0.25     | 80,000
$0.50     | 120,000
$1.00     | 250,000

##### Recurring Subscription Payments
Give your customers the ability to pay monthly with a dedicated QR code.

## Free Membership Limits
Ethereum POS provides free transaction forwarding with amounts under $10.00.

## Ethereum Fees

When the customer first submits the transaction, they will be forced to pay for the transaction fee which is around $0.02 USD. Once Ethereum POS has at least 12 confirmations on the customers transaction, the entire balance of the wallet will be sent to your Ethereum address that you inserted on the /new request.

For example, a customer sends $10.00. Most likely, the customer wallet will send around $10.02 to the Ethereum POS wallet. The Ethereum POS wallet will only receive $10.00 because of the $0.02 transaction fee. Once theres 12+ confirmation, Ethereum POS will send the entire wallet balance to your Ethereum address. You'll receive around $9.98, since the $0.02 transaction fee has been included.

<aside class="notice">
Ethereum POS does not take any Ethereum or gas during this transaction process.
</aside>

# Contract Information

Ethereum POS is currently in the process of making the transaction callback process a seamless and transparent process. The plan is to have the customer pay directly to Ethereum POS Contract with data to include your Ethereum address, order ID and callback information. Once you've collected your funds into your account inside of the contract, you'll be able to withdraw directly from contract, thus reducing the transaction fee each time a payment is transmitted.

1. Customer pays Etheruem address QR code
2. Ethereum POS sends entire balance with order data to Ethereum POS contract.
3. Merchant/Developer can hold funds inside of the contract and withdraw when requested. (sending a call to contract from requesting address)

<aside class="notice">
Email us:  info@ethereumpos.com
</aside>