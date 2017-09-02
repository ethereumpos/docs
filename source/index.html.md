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

Ethereum POS has a pretty basic transaction system.

1. Application creates new order for customer
2. Customer pays QR code Ethereum address
3. Successful alert for customer letting them know they paid. (expires in 15 minutes)
4. Application receives a POST to callback URL of order information and transaction ID.
5. Ethereum POS waits for 12 confirmations then sends entire wallet balance to your Ethereum address.
7. Another POST request to your application's callback URL.

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
