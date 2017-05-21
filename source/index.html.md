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

# Orders

## Create New Order

```shell
curl -X POST -H "Authorization: MYAPIKEY" -H "Content-Type: application/json" -d '{
    "amount": "13.54",
    "address": "0x3f05dc64b34f5063461f81d88972a3f542f57331",
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
  "currency_amount": "",
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
amount | true | Currency Value of order ($25.85)
address | true | Your Ethereum Address, payment will forward to.
callback | true | Callback URL for your application for this order.
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


# History

## Show All Orders

```shell
curl -X GET "https://api.ethereumpos.com/history"
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
curl -X GET "https://api.ethereumpos.com/history/<METHOD>"
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
    }
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


# System Information

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

# User Information

## Show Account Information

```shell
curl -X GET "https://api.ethereumpos.com/user"
```

```go
yoyoyooyoyoyoyo
```

> The above command returns JSON structured like this:

```json
{
	"id": 14,
	"address": "0x5be4c1b30534a6a30bf9f68ad6d3c0dc2a7fe292",
	"confirmations": 0,
	"currency": "usd",
	"currency_amount": "3.30",
	"expected_amount": "0.0681",
	"payment_url": "https://ethereumpos.com/payment/14",
	"status": "paid",
	"paid": true,
	"eth_price": "48.45",
	"qr_code": "http://chart.apis.google.com/chart?cht=qr&chs=500x500&chl=ethereum%3A0x5be4c1b30534a6a30bf9f68ad6d3c0dc2a7fe292&chld=H|0",
	"transaction_id": "0",
	"item_title": "New Item",
	"item_description": "Description",
	"item_image": "https://ethereumpos.com/images/ethereum.svg",
	"created_at": "0001-01-01T00:00:00Z"
}
```

This endpoint retrieves information about the merchant/developer account.

### HTTP Request

`GET https://api.ethereumpos.com/user`


Method Value | Description
--------- | -----------
pending | Customer has not sent this transaction yet.
success | Customer has sent the correct amount and has at least 1 confirmation
complete | Ethereum POS sent the entire wallet balance to the creators Ethereum address
expired | Customer didn't send the transaction within 15 minutes. This can also happen if customer sent a low transaction fee.


## Account Balance and History

```shell
curl -X GET "https://api.ethereumpos.com/user/history"
```

```go
yoyoyooyoyoyoyo
```

> The above command returns JSON structured like this:

```json
{
	"id": 14,
	"address": "0x5be4c1b30534a6a30bf9f68ad6d3c0dc2a7fe292",
	"confirmations": 0,
	"currency": "usd",
	"currency_amount": "3.30",
	"expected_amount": "0.0681",
	"payment_url": "https://ethereumpos.com/payment/14",
	"status": "paid",
	"paid": true,
	"eth_price": "48.45",
	"qr_code": "http://chart.apis.google.com/chart?cht=qr&chs=500x500&chl=ethereum%3A0x5be4c1b30534a6a30bf9f68ad6d3c0dc2a7fe292&chld=H|0",
	"transaction_id": "0",
	"item_title": "New Item",
	"item_description": "Description",
	"item_image": "https://ethereumpos.com/images/ethereum.svg",
	"created_at": "0001-01-01T00:00:00Z"
}
```

This endpoint retrieves information about the merchant/developer total income, outgoing, total transactions, etc.

### HTTP Request

`GET https://api.ethereumpos.com/user/history`


Method Value | Description
--------- | -----------
pending | Customer has not sent this transaction yet.
success | Customer has sent the correct amount and has at least 1 confirmation
complete | Ethereum POS sent the entire wallet balance to the creators Ethereum address
expired | Customer didn't send the transaction within 15 minutes. This can also happen if customer sent a low transaction fee.


# Pricing and Fees

## Memberships

Ethereum POSing has simple the understand billing

### Monthly Plans

Plan | Monthly Price | Daily Transactions | Max Amount (USD) | Recurring Payments
--------- | ------- | ------- | ------- | -----------
Free      |    $0.00    | 10   |  $10.00  |  no
Basic      |    $5.00    | 25   |  $30.00  |  no
Seller      |    $12.00    | 50   |  $100.00  |  yes
Premium      |    $20.00    | 150   |  $500.00  |  yes
Exclusive      |    contact us    | 500+   |  $1000.00  |  yes

### Plugin Features
Plugin | Monthly Price | Max Amount |
--------- | ------- | -----------
TXT Messaging 150      |    $5.00    | 150
TXT Messaging 500      |    $10.00    | 500
Recurring Payments      |    $10.00    | 500

#### Text Messaging
This Ethereum POS plugin feature is great for applications that would like to text the user or developer of recent payment activity.

##### Recurring Payments
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
