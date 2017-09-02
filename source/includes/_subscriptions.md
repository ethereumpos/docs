
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

<aside class="warning">
Requires the API Key Authorization Header. Replace <code>MYKEYHERE</code> with your API key.
</aside>

### HTTP Request

`POST https://api.ethereumpos.com/subscription`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
amount | true | Currency value of order in USD ($25.85)
callback | false | Callback URL for your application for this order.
recurring | true | This subscription is due every interval - see format

### Recurring Format
Recurring input is formatted in a simple method, put a number followed by a character to symbolize a interval. 
(d=day, w=week, m=month, y=year)
 
Example  | Description
-------- | -----------
7d | 7 days
3w | 3 weeks
6m | 6 months
1y | 1 year



## Create Subscription Order

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

This endpoint will create a new order for your subscription. 

<aside class="warning">
Requires the API Key Authorization Header. Replace <code>MYKEYHERE</code> with your API key.
</aside>

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

