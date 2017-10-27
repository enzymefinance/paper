<img src = "https://github.com/melonproject/branding/blob/master/melon/03_M_logo.jpg" width = "25%">

## Abstract

Automating fund administration and ensuring economic value stays in custody of the Investor at all time while given the Manager freedom to manage would allow for a new and efficient way of managing investment funds. A way where preagreed rules are enforced by technology and that is from central points of failure. 

Using distributed networks such as blockchains as a way of holding and transferring economic value provides one part of the solution. Another part is that economic value on distributed quasi turing complete machines can be held solely by smart-contract code and only be spent in a way which is pre-programmed within this code.
 
We propose a set of rules, called Melon protocol, for how such an investment fund could be created and operated in completely self-regulating, that is a technology regulated way. The rules are meant to protect both Investors and Managers from significant malevolent behavior regardless of whether the identity of the parties are known to each other.

The protocol implemented for example in Solidity or WASM acts as a template, called Melon fund, for how investment funds can be created. This template together with an frontend on how to access and operate the investment fund are designed in an reliable, permissionless and ownerless way.

Additionally we propose Melon chain, a customized distributed turing complete machine just for the purpose of having above investment funds operated in the most efficient, inter-connected and secure way possible.

## Table of Contents

* [Introduction](#Introduction)
* [Background](#Background)

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

Communication between the Investor and Manager can be achieved in a privacy preserving settings using Melon Mail.


## Investment Funds Design Overview

Broadly speaking a Melon fund should be capable of 

- accepting subscription and redemption requests, 
- make orders on an exchange
- take orders on an exchange
- receiving management and performance rewards


trade, take managment and performance rewards
Melon fund as a smart contract that acts as the accountant and custodian.

A Melon fund is created by triggering the Solidity function version.setupFund(..) on the latest protocol version contract.
Where the input parameters are
| Name | Unit | Description |

Melon boolean functions
- Participation
- Risk Management
Risk management can be seen as a state transistion function applied on the current holdings _h_ of the fund resulting into a new holdings state _h'_.
If _h'_ is outside of certain risk managment parameters the boolean function will return zero.

Melon libraries
- Rewards
	Management fee
	Performance fee
- ExchangeAdapter

Melon modules
- Asset Registrar
- Data Feed

Scenarios
- Invest in a fund
...
- Convert unclaimed rewards


## Melon Chain Design Overview

### Consensus

Open parachain
Linked into Polkadots pooled security
Makes it faster, cheaper and more secure since no additional validation process.

### State transition function

WASM vitrual machine together with a set of system contracts

### Governance

Representative Governance where Melon tokenholders elect chain Maintainers.




