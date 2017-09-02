# Pricing and Fees

## Memberships

Ethereum Point of Sale is extremly low cost for the customer and the developer/merchant. 
While you use Etheruem POS you can accept payments from any type of ETH wallet, even Coinbase. 
All pricing is measured in USD but will be converted in ETH when an order is placed.

### Monthly Plans

Plan | Price | Monthly Cost | Max (ETH) | Recurring Payments | Contract Calls
--------- | ------- | ------- | ------- | ----------- | --------
Basic     |    $0.25 per order    | $0.00   | 0.25 ETH |  yes  |  no
Seller    |    $0.15 per order    | $6.00   |  2 ETH   |  yes  |  no
Premium   |    $0.10 per order    | $25.00   |  10 ETH |  yes  | yes
Exclusive |    $0.00              | $50.00   |  100 ETH  |  yes  | yes
Corporate |    Contact Us             | ---  |  100+ ETH  |  yes  | yes

### Smart Contract Features
This fee includes all ETH transactions fees so you won't have to worry. Depending on how large your callback function is you can choose the gas limit.

Cost Per Call | Gas Limit |
--------- | -----------
$0.25     | 50,000
$0.30     | 80,000
$0.50     | 120,000
$1.00     | 250,000

##### Recurring Subscription Payments
Give your customers the ability to pay monthly with a dedicated QR code.

## Ethereum Fees

When the customer first submits the transaction, they will be forced to pay for the transaction fee which is around $0.02 USD. Once Ethereum POS has at least 12 confirmations on the customers transaction, the entire balance of the wallet will be sent to your Ethereum address that you inserted on the /new request.

For example, a customer sends $10.00. Most likely, the customer wallet will send around $10.02 to the Ethereum POS wallet. The Ethereum POS wallet will only receive $10.00 because of the $0.02 transaction fee. Once theres 12+ confirmation, Ethereum POS will send the entire wallet balance to your Ethereum address. You'll receive around $9.98, since the $0.02 transaction fee has been included.

<aside class="notice">
Ethereum POS does not take any Ethereum or gas during this transaction process.
</aside>

## Contract Information

Ethereum POS is currently in the process of making the transaction callback process a seamless and transparent process. The plan is to have the customer pay directly to Ethereum POS Contract with data to include your Ethereum address, order ID and callback information. Once you've collected your funds into your account inside of the contract, you'll be able to withdraw directly from contract, thus reducing the transaction fee each time a payment is transmitted.

1. Customer pays Etheruem address QR code
2. Ethereum POS sends entire balance with order data to Ethereum POS contract.
3. Merchant/Developer can hold funds inside of the contract and withdraw when requested. (sending a call to contract from requesting address)

<aside class="notice">
Email us:  info@ethereumpos.com
</aside>