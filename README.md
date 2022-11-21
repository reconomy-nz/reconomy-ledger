# Reconomy-ledger

Smart contracts for mutual credit ledger on Hyperledger Iroha 

## Context

Goal of mutual credit is to create communty resilience by fascilitating and encouraging exchange of local goods and services. For that it requires a private accounting system that is designed for the currency to circulate.

## Example

Let's put it into a real world example.

Let's say you and 100 local people got together and decided to create a cooperative and agreed to provide their goods and services withing this cooperative. All exchanges will be recorded in the private accounting system. Each member agreed to put 1000 credits of value into common account, therefore he becomes a stakeholder of the cooperative. Those credits can be redistributed and will circulate by the rules that cooperative will agree upon.
Within the cooperative members can create rules how to distribute, circulate, and use those credits. Those rules doesn't have to correlate or mimic any behaviour of real world economy. 

### Requirements 

* Accounting system should be designed to encourage velocity of exchanges and prevent hoarding.
* System should provide with a credit pool from where cooperative can fund community initiatives. As well as reward volunteers who's doing good deeds for community.

### Expectations

As a result on the local scale we will be expecting:

* Higher rates of utilisation of local services and produce.
* With increased velocity we'll expect more value created and exchanged within cooperative.
* Positive environmental impact.
* Community resilience. Increase of engagements within the local community will result in food security and emergency preparedness. 

## How

Here's more practical info how we can achieve it with Hyperledger Iroha.

### Variables
First we need to define few variables:

`AMOUNT_MINTED_PP` - Amount of minted credit per person
`TOTAL_AMOUNT_MINTED` - Total amount of minted credit in the network
`DEMURAGE_PRECENT_TIER_$X` - Precent of demurage payment for tier $X
`MINIMUM_DONATION_AMOUNT` - Minimum limit for 

### Logic
#### 1. Dynamic minting of credit.
Smart contract that mints `MINTED_AMOUNT_PP` of credits when new member joins.

#### 2. Redistribution
Minted credits then sent to distribution account that will redistributes credits into 4 different pools in following proportions:

70% Community pool. Community pool is dedicated to fund community initiatives dedicated to environment, education and social wellfare.

10% Rewards pool. To reward the most active and involved members of cooperative. The pool is capped at 10% of `TOTAL_AMOUNT_MINTED`. Excess to be sent to community pool.

10% Commitee pool. To support commitee members. To pay for organisationn of events, incubating projects, maintanance and functioning.

10% Balance pool. To burn credit when person leaves. The pool is capped at 10% of `TOTAL_AMOUNT_MINTED`. Excess to be sent to community pool. 

#### 3. Reccuring donation
To recirculate credits and provide credits to refill pools
By joinng cooperative members agree to contribute a monthly donation, which has a minimum limit of `MINIMUM_DONATION_AMOUNT`. This payment to be sent into distribution account. This donation serves a purpose to recirculate credits and refill pools. Reccuring payment will be set when members engage with the system for the first time. Further members can alter the amount of donation.

#### 4. Demurage.


