---
id: approve-all
title: approveAll
keywords:
- 'pos client, erc721, approveAll, polygon, sdk'
description: 'Aprubahan ang lahat ng token.'
---

Maaaring gamitin ang `approveAll` upang aprubahan ang lahat ng token.

```
const erc721RootToken = posClient.erc721(<root token address>, true);

const approveResult = await erc721RootToken.approveAll();

const txHash = await approveResult.getTransactionHash();

const txReceipt = await approveResult.getReceipt();

```
