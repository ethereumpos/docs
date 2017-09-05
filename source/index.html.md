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
  - orders
  - subscriptions
  - history
  - contracts
  - pricing
  - errors

search: true
---

# Introduction

Welcome to the Ethereum POS (Point of Sale) API Documentation! This documentation covers everything you need to know about accepting ethereum for your application. You'll be able to create orders, retrieve order information, and get your account history.


### Payment Processing Steps
Ethereum POS has a pretty basic transaction system.

1. Your website or application sends a request to API for a specified USD value for their order.

2. Your application shows a QR code for Ethereum Address and expected ETH value to send. 

3. The customer pays in ETH and the ordered is marked as 'paid' and sends a callback to your website or ethereum smart contract confirming the order was paid.

4. The ETH funds that were sent from customer gets transferred to the merchant/developer. $0.25 USD is sent to Ethereum POS.


### Example of Transaction

[Order Wallet](https://ropsten.etherscan.io/address/0xff64a0bd49de1dd49d310faca6e339601982a945)

[Customer Payment](https://ropsten.etherscan.io/tx/0x9b0ef0428bbde40924d728d487ddb41e9ea14a3fc62ab550f747fb1c8717750e)

[Payment to Merchant](https://ropsten.etherscan.io/tx/0xe1e7a1a32c88dfb2869a9856401c3d319322d37872f678d4e57a852c5d116cbd)

[Ethereum POS Payment](https://ropsten.etherscan.io/tx/0x1442c843425950c5a0e31df23da520a8103312314a9c71edaebfcb20081a9bc3)


<aside class="notice">
BETA API - Please use the Testnet API Endpoint first.
</aside>

We have language bindings in cURL, Golang, and in jQuery. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

Ethereum POS is currently in the process of making the transaction callback process a seamless and transparent process. The plan is to have the customer pay directly to Ethereum POS Contract with data to include your Ethereum address, order ID and callback information. Once you've collected your funds into your account inside of the contract, you'll be able to withdraw directly from contract, thus reducing the transaction fee each time a payment is transmitted.

## API Endpoints
Ethereum Point of Sale works on Ethereum's Mainnet (ETH) for production use. When you want to test your application, use the Testnet API that runs on the Ropsten Ethereum blockchain.

### Production API Endpoint (Ethereum Blockchain ETH)
`https://api.ethereumpos.com`

### Testing API Endpoint (Ethereum Ropsten Testnet)
`https://testapi.ethereumpos.com`

# Authentication

Ethereum POS uses API keys to allow access to the API. You can register a new API key by registering on [ethereumpos.com](https://ethereumpos.com).

API requests to the server as a HEADER will look like the following:

`Authorization: MYKEYHERE`

<aside class="notice">
You must replace <code>MYKEYHERE</code> with your personal API key.
</aside>
