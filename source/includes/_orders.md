
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

This request will create a new order for your customer to send ETH to. Each new request creates a new ethereum wallet. 
Once the order is paid in full and confirmed the ether is forwarded to your ethereum wallet address. 
This address is set when you registered for ethereumpos.com and is available in your dashboard.

### Expiration

If an order does not get paid within 50 blocks it will expire. An expiration is needed since ETH vs USD changes often. 
Once an order is expired, the order cannot be paid to. Create a new order to update the pricing relative to ether.

<aside class="warning">
Requires the API Key Authorization Header. Replace <code>MYKEYHERE</code> with your API key.
</aside>

### HTTP Request

`POST https://api.ethereumpos.com/order`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
amount | true | Currency value of order in USD ($25.85)
callback | false | Callback URL or Ethereum Smart Contract address
ref_id | false | Order ID based on your website or application (optional)
item_title | false | The item title to be shown to user (optional)
item_description | false | The item description for user (optional)
item_image | false | The item image to display to user (optional)

### Using Smart Contract for Callback

Rather than using a HTTP callback, you can use an ethereum contract address and Ethereum POS 
will do the callback() function on your contract with order information. 


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

This API request will respond with the order information. 
You can send this request if you need to update the stats of an order on your application. The request includes the same JSON structure as creating a new order.

### HTTP Request

`GET https://api.ethereumpos.com/order/<ID>`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
ID | true | Ethereum POS Order ID

