---
id: index
title: Welcome to zkEVM
hide_title: true
sidebar_label: Welcome
description: zkEVM Wiki is the documentation hub for Polygon zkEVM, providing extensive documentation, community resources, and guides for building on zkEVM.
keywords:
  - docs
  - zkEVM
  - wiki
  - polygon
image: https://wiki.polygon.technology/img/thumbnail/polygon-zkevm.png
---

import Welcome from "./landing.js";

:::info Before We Begin

Polygon zkEVM Mainnet Beta is provided on an **AS-IS** and **AS-AVAILABLE** basis. The documentation contains statements about technical specifications, some of which may relate to future versions of Polygon zkEVM rather than its current implementation.

:::

<Welcome/>

---

### Polygon zkEVM Risk Disclosures

<details>
<summary>Attack Vectors/Security</summary>

- This is a **Mainnet Beta** and **not a Mainnet release of Polygon zkEVM**, [<ins>security audits</ins>](https://polygon.technology/blog/polygon-zkevm-results-of-hexens-security-audit) and assessments are ongoing. Your data and crypto-assets may be at risk as a result of bugs or otherwise.

- **Polygon zkEVM technology is novel**. As such, there may be unanticipated issues and risks associated with your use of the technology. For example, there may be errors that result in losing data or crypto-assets.

- **Cross-blockchain bridging may be subject to cyberattacks and exploits** including, without limitation, hacks that exploit a vulnerability in the software, hardware, systems or equipment associated with any bridge component, smart contracts, and related systems.

</details>

<details>
<summary>Network Availability/Performance</summary>

As this is a Mainnet Beta, Polygon zkEVM may be slow or unavailable from time to time without notice, which could result in unexpected loss of use or data or crypto-assets. Before engaging in high value transactions, be mindful that there may be time delays before transactions are finalized.

</details>

<details>
<summary>Decentralization Progress</summary>

Polygon Labs is in the process of further decentralizing Polygon zkEVM. This refers to the process of gradually increasing decentralization of the system over time.

- The Mainnet Beta will have some centralized features, such as the Sequencer and Aggregator (Prover), that Polygon Labs currently maintains in an effort to provide greater security at this time. **The Sequencer has the ability to delay the inclusion of a transaction and otherwise reorder transactions**.

- Security of Polygon zkEVM Mainnet Beta is a continuous process. This process includes responding to security concerns, which depends on the Security Council. **The Security Council consists of 7 individuals who are empowered to upgrade Polygon zkEVM Mainnet Beta** without a timelock to respond to urgent security issues. If members of the Council behave maliciously or collude, then the integrity of the system may be compromised including network upgrades that may result in loss of crypto-assets.

- As the Sequencer and Aggregator are centralized for Mainnet Beta, there are risks for potential network downtime and outages, including those that are outside the control of Polygon Labs.

- During the initial phase of the Mainnet Beta release, users will not be able to force transactions on Layer 1.

</details>

<details>
<summary>Gas Fees</summary>

If the gas fees associated with a proposed transaction are too low, it is possible that such transaction will not be sequenced and that those fees may be lost.

</details>

<details>
<summary>Security Audits</summary>

- Polygon Labs’ implementation of Polygon zkEVM has been carefully constructed, was audited by several internal and external parties, and is continuously being reviewed and tested against engineering best practices. It is, however, unlikely that all potential bugs or vulnerabilities were identified through these audits and thus there may be undiscovered vulnerabilities that may put user funds at risk. Users should consider this risk when deciding how much value to place onto the Polygon zkEVM Mainnet Beta. To see the audit reports, see [<ins>here</ins>](https://github.com/0xPolygonHermez/zkevm-rom/blob/main/audits/Hexens_Polygon_zkEVM_PUBLIC_27.02.23.pdf).

- There is a robust bug bounty program for Polygon zkEVM to help encourage the community to find critical bugs in the codebase. Head over to the [<ins>Polygon zkEVM Bug Bounty page on Immunefi</ins>](https://immunefi.com/bounty/polygonzkevm/).

</details>

<details>
<summary>Prover Infrastructure</summary>

- Currently the Polygon zkEVM zkProver does not run on ARM-powered Macs. For Windows users, using WSL/WSL2 is not recommended. Apple M1 chips are not supported for now, since some optimizations on the zkProver require specific Intel instructions. This means some non-M1 computers won't work regardless of the OS, for example: AMD.

- In the event you are deploying a full node of Polygon zkEVM Mainnet Beta, be mindful that the network data is stored inside of each docker container. This means once you remove the container that network data will be lost and you will be required to resync the network data.

</details>