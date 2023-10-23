---
title: "Public and Managed Blockchains"
order: 4
description: Introduction to different deployment patterns
tags:
  - concepts
---

# Public and Managed Blockchains

The most obvious way of operating blockchain protocols comes in the form of a public network, facilitating reliable **peer-to-peer (P2P)** transactions between individual participants who may not know or trust each other.

![P2P network](/ida-course/0-blockchain-basics/images/00_02_p2p_network_dark.png)

This is what blockchain technology was originally invented for and remains arguably its most powerful use - but it is not necessarily the only application.

One of the biggest expenses that business (and other) institutions face is the operation and maintenance of infrastructure and the costs resulting from leaks, hacks, reconciliation with trading partners, errors, and data incompatibility. Blockchain seems like a promising solution to managing these concerns, but doing so through the use of a fully public network may well present an insurmountable obstacle.

The solution? To adapt the original, purely public use case of blockchain technology to a private, or "managed", version. This section will examine the differences of these approaches.

## Public Blockchains

A public blockchain network has a few specific attributes:

* **Accessibility:** All you need to connect to Bitcoin or Ethereum is the client software and an internet connection. No AML, KYC, identity checks, or subscription payment is required.
* **No hierarchy:** All nodes are equal, meaning no individual node has more authority than another. All validators are also equal.
* **Crypto-economic incentives:** The lack of a central authority means there is no absolute defense against malicious behavior. Instead, the network usually incentivizes benevolent behavior and disincentivizes behavior that endangers the network functioning. It de facto implements prohibitive expenses to attack the network and others, and thus ensures security and proper function.
* **Full decentralization:** Many public networks are completely decentralized because they are non-hierarchical and fully accessible. The playing field for market participants is therefore relatively level, so traditional business models may not work as well.

The two most popular examples of functioning public networks are Bitcoin and Ethereum.

### Introduction to Bitcoin

Since 2009, the most successful and popular decentralized public blockchain network has been Bitcoin. Nowadays, Bitcoin is the cryptocurrency with the highest market capitalization.

<HighlightBox type="tip">

For an estimate of the Bitcoin network size, take a look at [Bitnodes](https://bitnodes.earn.com/). If you are more of a visual and statistics person, these [Bitcoin charts](https://www.blockchain.com/en/charts) are interesting.

</HighlightBox>

Bitcoin was first conceived as a P2P version of electronic cash. **Proof-of-work (PoW)** was proposed to establish the truth in a partially synchronous system without involving trusted parties. Using this method, the set of participants controlling the majority of the computing power determines the truth. This powerful system deserves a brief closer examination.

Each block in Bitcoin includes the hash of the previous block and a "nonce", which is an additional value that produces a hash for the new block that begins with a specific number of binary `0`s. Bitcoin uses a timestamp server to prevent double-spending.

![Chain of blocks with previous hash](/ida-course/0-blockchain-basics/images/00_16_bitcoin_block_headers_literal.png)

The block creator, called a **miner**, looks for a nonce which satisfies this requirement. A valid nonce is difficult to find, basically random trial and error, but easy to verify after the fact, which is how the network checks and accepts new blocks. The protocol includes a reward for **mining**, and it can be expected that the majority of nodes use their CPU power honestly because this is the most financially feasible course of action. Signed transactions are announced publicly, so the public keys of the parties are not private.

![Mining](/ida-course/0-blockchain-basics/images/00_17_mining-01.png)

The longest valid chain of blocks on the network is deemed the truth, because users recognise it contains proof of the most work done to create (hence "proof of work"). It becomes exponentially more difficult to alter history as more blocks are appended. To create a rival valid chain which is at least as long as that used by the consensus, an attacker must find a new nonce for the block they want to change **plus** new nonces for _every subsequent block thereafter_. This pits the attacker against the combined brute force (i.e. computing power) of the rest of the network.

There is a residual possibility that a slower attacker can catch up, since discovering a nonce works by evaluating random numbers and it is possible to make a fast lucky guess. However, this probability decreases exponentially as the number of blocks increases, so making changes to the "distant" past becomes progressively less practical. This is Bitcoin in a nutshell.

### Introduction to Ethereum

Ethereum is a public, blockchain-based, distributed computing platform and an operating system with **smart contract** features.

Ethereum emerged from a range of proposals rejected by the Bitcoin community. Ethereum recently transitioned from Bitcoin's Proof-of-Work to use Proof-of-Stake consensus. You can find out more about different consensus models [in the next section](./4-consensus.md).

The most important difference Ethereum introduces is the implementation of distributed code execution, which happens through the **Ethereum Virtual Machine (EVM)**. The Ethereum network is a virtual state machine which enables the deployment of so-called smart contracts as part of the data of a transaction. These smart contracts are stored on every single node - they are public. In practice, a smart contract in the EVM is an autonomous agent with an internal account. The most popular language for writing such a contract is **Solidity**.

The implementation of such a state machine with blockchain is revolutionary in itself. The code execution platform is [Turing-complete](https://en.wikipedia.org/wiki/Turing_completeness), which means it must overcome the [halting problem](https://www.scientificamerican.com/article/why-is-turings-halting-pr/), something that is especially difficult in distributed, hierarchy-free computing platforms.

In simple terms, the halting problem describes a scenario in which a program loops forever. Ethereum's solution is to introduce financial costs for each computational step. Transactions that do not compute successfully and are not on budget are rejected.

Every block has a maximum **Gas** limit which limits the number of computational steps that can be executed per block - just like a combustion engine, if the Gas runs out then the program stops. 

<HighlightBox type="info">

Gas only exists during the execution of a transaction. The user is free to specify any **Gas price** in terms of **Ether**, the native cryptocurrency of the Ethereum platform, by setting a value which Ethereum requires to convert **Ether to Gas**.

</HighlightBox>

The fact that Ethereum's virtual machine can run arbitrary programs hints at the potential of blockchain technology to disintermediate and decentralize processes which include network participants that would benefit from a shared set of facts and reliable interactions. The field of possibilities is expansive. It includes virtually all cases where trading partners need to reconcile their respective records, trade assets, and considerations in an atomic way, or to execute remedial actions such as penalties if a counterparty fails to deliver as agreed.

As promising as this sounds, **this technology is not without limitations**. Chief among these limitations is capacity, not only in terms of transactions per second but also the complexity of transactions that can be handled by the network. Ethereum places limits on transaction complexity to ensure that no single contract or transaction overloads the shared, distributed computer. This constraint is inherent to the design choice of using a virtual machine model.

## Managed blockchain networks

Managed networks, just like public networks, rely on blockchain data structures. Unlike public blockchain networks, they typically operate in a predictable environment with elements of authority, hierarchy, and accountability. Managed networks can be used in cases where elements of trust already exist between the participant, and are typically governed through traditional governance processes that are appropriate for the shared goals of the participants.

Consider a network of financial institutions. They could use blockchain technology to settle inter-bank transactions. In that case, there would be no need for public access, as in the Bitcoin model; indeed, public access would be undesirable.

Instead, they might consider a private network very convenient, one in which all participants understand that no operator in a single institution would be able to corrupt the network overall.

However, unlike the exponential challenge that impedes altering the transaction history of a _public_ network, in a _managed_ network it would be much easier for the institutions _as a group_ to alter a past record if they found it detrimental. Doing so would only require the coordinated action of a small number of known validators, instead of the agreement of a vast independent network whose participants are not known to each other.

**Private or managed blockchains** can be:

* **Designed for a limited number of vetted and approved participants:** performance challenges and poorly connected nodes are of lesser importance.
* **Designed for optimized performance:** most participants in an enterprise network are capable of running well-connected, high-performance, and high-availability nodes. A group of participants can agree to raise the bar defining minimum system requirements significantly.
* **Governed by a well-defined agreement between the participants:** such an agreement may codify the decision-making processes used to decide matters such as protocol upgrades, admission requirements, and remedial action. In a private or consortium setting, "who decides?" can (or likely _must_) be determined well in advance of any incident.

## Comparing public and private networks

The evolution of networking began with the traditional system of centralized control and now includes the decentralization offered by blockchain, either as a "permissionless" public network or a "permissioned" private or managed network.

![Public and managed network comparison](/ida-course/0-blockchain-basics/images/00_08_public_vs_private_comparison-01.png)

Public networks are based on game theory and economic incentives, which means that every action is probabilistic. There is no guarantee that a transaction will be picked up and even the integrity of the network is merely very likely, not 100% guaranteed.

Again, traditional financial institutions would likely find this unacceptable. However, unlike public networks where the interaction between participants is governed by the protocol and crypto-economic incentives, in managed networks the blockchain protocol is often a technical enforcement of pre-existing relationships and legally enforceable agreements.

![Public vs private](/ida-course/0-blockchain-basics/images/00_10_public_vs_private_comparison_table.png)

In summary, managed networks enable high-performance blockchain networks that can use consensus processes which are not suited for an environment with anonymous users. A group of trading partners can create a small network for their purposes and agree on equitable participation in the block-generation process (e.g. that each participant runs one validator), minimum performance metrics for acceptable validators, and governance, all of which enable fast confirmation and even deterministic transaction finality within their small group. The principal trade-off for this performance improvement is the shunning of permissionless, public access.

## How the interchain is different

The Cosmos SDK enables the creation of application-specific blockchains. Cosmos blockchains do not need smart contracts, because application-level concerns are defined at the protocol level. This offers developers the possibility of enabling transactions with complexity far above what is possible on general-purpose blockchains.

Because Cosmos chains can interact with other public networks (such as Ethereum) through the Inter-Blockchain Communication Protocol (IBC), Cosmos blockchains can also be used to offload processes that are either too complex or too expensive to run on other networks.

The result is that the interchain can be applied to both public and private settings and, importantly, supports communication between networks following different consensus rules, a seemingly intractable challenge for the predecessors of the interchain.

<HighlightBox type="reading">

**Further reading**

* [Buterin, Vitalik (2014): A Next-Generation Smart Contract and Decentralized Application Platform - The Ethereum White Paper](https://github.com/ethereum/wiki/wiki/White-Paper)
* [Vitalik Buterin on private chains](https://blog.ethereum.org/2015/08/07/on-public-and-private-blockchains)
* [Permissioned blockchains in production](https://www.multichain.com/blog/2017/11/three-non-pointless-blockchains-production/)

</HighlightBox>

<HighlightBox type="synopsis">

To summarize, this section has explored:

* How *public* blockchain networks tend to emphasize the characteristics of _accessibility_, _the absence of hierarchy_, _full decentralization_, and the use of _crypto-economic incentives_. They are based on game theory and economic rewards and are probabilistic in nature, so they cannot absolutely guarantee transaction inclusion or event network integrity, which makes them problematic for some traditional institutions.
* How *managed* blockchain networks offer an alternative as they do not necessarily need to mitigate the Byzantine Generals Problem, because they operate in predictable environments with elements of authority, hierarchy, and accountability. They often embrace traditional governance processes appropriate to the shared goals of the participants.
* How **Bitcoin** extends its record of network activity by rewarding independent "miner" nodes for grouping new transactions into blocks with particular characteristics that require much work to achieve but little work to verify. These blocks are constructed into a chain such that any alteration of data is easily identifiable, and any deliberate manipulation of the historical record would require unfeasible efforts on the part of an attacker to achieve.
* How the **Ethereum** network provides a revolutionary blockchain-based, Turing-complete virtual state machine (the EVM), delivering a computing platform and operating system with smart contract features. To avoid the halting problem, in which programs become stuck in an endless loop, Ethereum introduced the notion of charging a fee for each computational step, meaning that programs with insufficient "gas" to complete will simply stop, releasing the EVM to work on other programs.
* How the interchain moves past the example set by Ethereum to offer developers an ecosystem capable of platforming smart contracts or dApps _individually_, as opposed to forcing them to share and conform to the technological constraints of the single Ethereum blockchain. It can satisfy the demands of both public and private blockchains, and also supports communications between networks even if they follow differing consensus rules.

</HighlightBox>
