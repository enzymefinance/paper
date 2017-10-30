<img src = "https://github.com/melonproject/branding/blob/master/melon/03_M_logo.jpg" width = "25%">

## Abstract

Automating fund administration and ensuring economic value stays in custody of the Investor at all time while given the Manager freedom to manage would allow for a new and efficient way of managing investment funds. A way where preagreed rules are enforced by technology and that is from central points of failure. 

Using distributed networks such as blockchains as a way of holding and transferring economic value provides one part of the solution. Another part is that economic value on distributed quasi turing complete machines can be held solely by smart-contract code and only be spent in a way which is pre-programmed within this code.
 
We propose a set of rules, called Melon protocol, for how such an investment fund could be created and operated in completely self-regulating, that is a technology regulated way. The rules are meant to protect both Investors and Managers from significant malevolent behavior regardless of whether the identity of the parties are known to each other.

The protocol implemented for example in Solidity or WASM acts as a template, called Melon fund, for how investment funds can be created. This template together with an frontend on how to access and operate the investment fund are designed in an reliable, permissionless and ownerless way.

Additionally we propose Melon chain, a customized distributed turing complete machine just for the purpose of having above investment funds operated in the most efficient, inter-connected and secure way possible.

## Table of Contents

* [Introduction](#introduction)
* [Background](#background)
* [Investment Funds Design](#investment-funds-design)
* [Melon Chain Design](#melon-chain-design)

## Introduction

## Background

### Decentralized Execution (Melon chain)

Quasi turing complete machines such as Ethereum allow for computer code to be executed on a distributed basis within the constraints of a preagreed protocol.
Initially introduced by Ethereum this concept how evolved into Polkadot. A multichain framework allowing for exchange of information accross many different chains with different characteristics.

#### Assets

An example for such computer code is known as the ERC20 standart. Essentially a small (<100 lines of code) piece of software implementing a bitcoin-like cryptocurrency.

#### Exchanges

Another example are exchanges. Given above concept one now can implement computer code one how exchange of above assets can be facilitated in a decentralised way.

#### Investment Funds

Using the concept that smart contracts can be custodian of assets, we can now build smart contracts that act as fully functional investment funds. 

### Decentralized Hosting (IPFS)

The content addressed nature makes it more secure.

Eventually hosting of the Melon application will be payed by services such as client-side mining. Meaning as long as there is a demand for the application, it will be continuously hosting within the bittorrent swarm of IPFS.

### Privacy Preserving Communication (Melon Mail)

Melon Mail is a mail service that stores your private data on the distributed web (ipfs). Which means that no one owns your data other that you. Encrypted with the same cryptographic principles that make your crypto-currencies secure

Therefore communication between the Investor and Manager can be achieved in a privacy preserving way using Melon Mail.

## Investment Funds Design

### Governance

<img src = "https://github.com/melonproject/branding/blob/master/explanation/governance.png" width = "100%">

The main functionality of the [Governance](src/system/Governance.sol) contract is to add new protocol _versions_ such as this [Version](src/version/Version.sol) contract and to shut down existing versions once they become obsolete.

Adding new protocol version is done by anyone _proposing_ a version to be added and is _executed_ once authority consensus has been established.

Shutting down an existing protocol version is done by anyone _proposing_ a version to be shut down and is _executed_ once authority consensus has been established.

Shutting down a version disables the ability to setup new funds using this version and enables anyone to shut down any existing funds of this version.

### Investment Funds

Melon fund as a smart contract that acts as the accountant and custodian.
Broadly speaking a Melon fund should be capable of 

- accepting/executing subscription and redemption requests, 
- make orders on an exchange
- take orders on an exchange
- receiving management and performance rewards

A Melon fund is created by triggering the Solidity function version.setupFund(P) on the latest protocol version contract.
Where the input parameters _P_ are:

Name | Unit | Description
--- | --- | ---
withName | `string` | Human-readable describive name (not necessarily unique)
ofReferenceAsset | `string` | Asset against which performance reward is measured against§
ofManagementRewardRate | `address` | A time based reward, given in a number which is divided by 10 ** 15
ofPerformanceRewardRate | `uint` | A time performance based reward, performance relative to ofReferenceAsset, given in a number which is divided by 10 ** 15

### Modules

<img src = "https://github.com/melonproject/branding/blob/master/explanation/fund.png" width = "100%">

### List of Melon modules

Melon has six different module classes:
- Exchange Adapters
- Rewards
- Participation
- Risk Management
- Asset registrars
- Data feeds

Which can be categorized into three sub sets:
- Libraries
- Boolean function
- Infrastructure

#### Libraries

They interact with the Melon protocol using as pre-linked libraries to the Melon version contract. These Melon modules are:

- **Exchange Adapters**:
These are adapters between Melon protocol and decentralized exchanges. Responsible for relaying information, making and taking orders on exchanges.

- **Rewards**:
This module defines functions for calculating management and performance rewards for the fund manager. Management reward is calculated on the time managed irrespective of performance of the fund while performance reward is calculated based on the performance.

#### Boolean functions

They interact with the Melon protocol using boolean functions. That is functions which take a certain set of inputs and return either true or false. These Melon modules are:

- **Participation**:
It comprises of two primary boolean functions isSubscriptionPermitted and isRedemptionPermitted which enforce rules for investing and redemption from the fund. They take the parameter inputs as specified in the earlier section.

The Participation module takes as input the following parameters:

**Requests:** Describes and logs whenever asset enter and leave fund due to Participants

Name | Data Type | Description
--- | --- | ---
participant | `address` | Participant in Melon fund requesting subscription or redemption
status | `enum` | Enum: active, cancelled, executed; Status of request
requestType | `enum` | Enum: subscribe, redeem
shareQuantity | `uint256` | Quantity of Melon fund shares
giveQuantity | `uint256` | Quantity in Melon asset to give to Melon fund to receive shareQuantity
receiveQuantity | `uint256` | Quantity in Melon asset to receive from Melon fund for given shareQuantity
incentiveQuantity | `uint256` | Quantity in Melon asset to give to person executing request
lastDataFeedUpdateId | `uint256` | Data feed module specific id of last update
lastDataFeedUpdateTime | `uint256` | Data feed module specific timestamp of last update
timestamp | `uint256` | Time of request creation

- **Risk Management**:
It currently comprises of two boolean functions isMakePermitted and isTakePermitted. They take the parameter inputs as specified in the earlier section. This module can be extended with custom logic to prevent malevolent actions by the fund manager. This may include checks if the order price is significantly different from the reference price, e.t.c.

Risk management can be seen as a state transistion function applied on the current holdings _h_ of the fund resulting into a new holdings state _h'_.
If _h'_ is outside of certain risk managment parameters the boolean function will return zero.

The Risk Management module takes as input the following parameters:

**Orders:** Describes and logs whenever assets enter and leave fund due to Manager

Name | Data Type | Description
--- | --- | ---
exchangeId | `uint256` | Id as returned from exchange
status | `enum` | Enum: active, partiallyFilled, fullyFilled, cancelled
orderType | `enum` | Enum: make, take
sellAsset | `address` | Asset (as registered in Asset registrar) to be sold
buyAsset | `address` | Asset (as registered in Asset registrar) to be bought
sellQuantity | `uint256` | Quantity of sellAsset to be sold
buyQuantity | `uint256` |  Quantity of sellAsset to be bought
timestamp | `uint256` | Time in seconds when this order was created
fillQuantity | `uint256` | Buy quantity filled; Always less than buy_quantity


#### Infrastructure

These are security critical infrastructure modules. These Melon modules are:

- **Asset registrars**:
It is a chain independent asset registrar module. Only the registered assets will be available. Cross-chain asset integration is enabled through Polkadot in future. An asset can be registered via the register function by specifying the following parameters:

Name | Data Type | Description
--- | --- | ---
asset | `address` | Address of the asset
name | `string` | Human-readable name of the Asset as in ERC223 token standard
symbol | `string` | Human-readable symbol of the Asset as in ERC223 token standard
decimal | `uint` |  Decimal, order of magnitude of precision, of the Asset as in ERC223 token standard
url | `string` | URL for extended information of the asset
ipfsHash | `bytes32` | Same as url but for ipfs
chainid | `bytes32` |  Chain where the asset resides
breakIn | `address` | Break in contract on destination chain
breakOut | `address` | Break out contract on this chain

- **Data feeds**:
Data feeds route external information such as asset prices to Melon fund smart contracts.

The reason they are security critical is that the correctness of the data they provide cannot directly be enforced or guaranteed and trust is placed on a central authority.

## Interaction

Smart contract interaction for the following scenarios:
- Setup of a new Melon fund
- Participant invests in a Melon fund
- Participant redeems from a Melon fund
- Manager makes an order
- Manager takes an order
- Manager converts rewards into shares
- Manager shuts down the fund

**Setup of a new Melon fund**

A new Melon fund can be setup by specifying the following parameters via setupFund function of the version contract:

Name | Data Type | Description
--- | --- | ---
name | `string` |A human readable name of the fund
referenceAsset | `address` | Asset against which performance reward is measured against
managementRewardRate | `uint` | Reward rate in referenceAsset per delta improvement
performanceRewardRate | `uint` | Reward rate in referenceAsset per managed seconds
participation | `address` | Participation module
riskMgmt | `address` | Risk management module
sphere | `address` | Sphere module

**Participant invests in a Melon fund**

1. A participant starts to invest in a fund F by first creating a subscription request R. Parameters to be specified are:

Name | Data Type | Description
--- | --- | ---
giveQuantity | `uint` | Quantity of Melon tokens to invest
shareQuantity | `uint` | Quantity of fund shares to receive
incentiveQuantity | `uint` | Quantity of Melon tokens to award the entity executing the request

2. R parameters are checked against restriction rules specified in the participation module P by the boolean function P.isSubscriptionPermitted (E.g Participant being an attested Uport identity).
3. R is then executed in by any entity via F.executeRequest after certain conditions are satisfied.  These conditions include if *currentTimestamp - R.timestamp >= DF.INTERVAL* (DF refers to datafeed module and INTERVAL corresponds to update frequency value) and if *DF.getLastUpdateId >= R.lastDataFeedUpdateId + 2*. This is to minimize unfair advantage from information asymmetries associated with the investor.

**Participant redeems from a Melon fund**

1. A participant can redeem from a fund by first creating a redemption request. Parameters to be specified are:

Name | Data Type | Description
--- | --- | ---
shareQuantity | `uint` | Quantity of fund shares to redeem
receiveQuantity | `uint` | Quantity of Melon tokens to receive in return
incentiveQuantity | `uint` | Quantity of Melon tokens to award the entity executing the request

2. Request parameters are checked against restriction rules specified in the participation module P via the boolean function P.isRedemptionPermitted.
3. R is then executed in a similar way as mentioned earlier.

**Manager makes an order**

1. Manager can make an order by specifying asset pair, sell and buy quantities as parameters. Asset pair is checked against datafeed module DF through the function DF.existsData. Order parameters are then checked against restriction rules specified in the risk management module R via the boolean function R.isMakePermitted.
2. The specified quantity of the asset is given allowance to the selected exchanging via ERC20's approve function.
3. Order is then placed on the selected exchange through the exchangeAdapter contract E via E.makeOrder by specifying the exchange and order parameters as parameters.
4. The order is filled on the selected exchange (In future, can be any compatible decentralized exchange like OasisDex, Kyber, e.t.c)  when the price is met.

**Manager takes an orders**

1. Manager can take an order by specifying an order id and quantity as parameters. Asset pair is checked against datafeed module DF through the function DF.existsData. Order parameters are then checked against restriction rules specified in the risk management module R via the boolean function R.isTakePermitted.
2. The specified quantity of the asset is given allowance to the selected exchanging via ERC20's approve function.
3. Order id must correspond to a valid, existing order on the selected exchange. Order is then placed on the selected exchange through the exchangeAdapter contract E via E.takeOrder by specifying the exchange and order parameters as parameters.

**Manager converts rewards into shares**

1. Manager rewards in the form of ownerless shares of the fund F can be allocated to the manager via F.convertUnclaimedRewards function. Ownerless shares refer to the quantity of shares, representing unclaimed rewards by the Manager such as rewards for managing the fund and for performance. First internal stats of F are calculated using F.performCalculations function. The quantity of unclaimedRewards is calculated internally using calcUnclaimedRewards function.
2. A share quantity of *unclaimedRewards * gav* (from Calculations) is assigned to the manager.

**Manager shuts down the fund**

1. A Manager can shut down a fund F he owns via F.shutdown function.
2. Investing, redemption (Only in reference asset, investors can still redeem in the form of percentage of held assets), managing, making / taking orders, convertUnclaimedRewards are rendered disabled.

ofParticipation | `address` | Address of participation module
ofRiskMgmt | `address` | Address of risk management module
ofSphere | `address` | Address of sphere, which contains address of data feed module


## Melon Chain Design

### Consensus

Open parachain
Linked into Polkadots pooled security
Makes it faster, cheaper and more secure since no additional validation process.

### State transition function

WASM vitrual machine together with a set of system contracts

### Governance

Representative Governance where Melon tokenholders elect chain Maintainers.




