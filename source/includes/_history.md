

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

This API request lets you view each order you have with EthereumPOS.com.

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

This API request will respond back with an array of orders based on the order method status you requested.

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

This API request responds useful information to help your application know what's going on with EthereumPOS.com API and blockchain status.

### HTTP Request

`GET https://api.ethereumpos.com/status`

### Response Values

Value | Description
--------- | -----------
block | the most up to date block with Ethereum and our servers.
test | will be set to true if request is sent to test API.
usd | USD pricing of ETH
eur | EURO pricing of ETH
gbp | GBP pricing of ETH

