# Reconomy-ledger

Smart contracts for mutual credit ledger based on Hyperledger Iroha 

## Context

Mutual credit is an alternative currency that is used within a community or group of people. It is based on the principle of reciprocal exchange, in which members of the community agree to trade goods and services with each other using units of credit that are issued and managed by the group. Unlike traditional currencies, which are issued and controlled by central banks or governments, mutual credit is issued and managed by the community itself.

The goal of mutual credit is to create an alternative means of exchange that allows members of a community to trade goods and services with each other without relying on traditional forms of money. Mutual credit systems are designed to facilitate trade among members of the community, support local businesses, and promote economic self-sufficiency. Mutual credit systems do not intend to serve as a means of storing value and have measures in place to discourage hoarding. 

## Problem statement
In today's global economy, social and financial inequality are widespread and growing, with a few large corporations dominating the free market and driving out smaller, local businesses. This concentration of power and wealth not only exacerbates the divide between the rich and the poor but also undermines the principles of a fair and healthy market system. The current economic model is unsustainable and inequitable, and it has contributed to a separation that has resulted in an individualistic mindset and a lack of community. Additionally, the broken reciprocity between people and nature, characterized by a lack of respect and care for the natural world, is causing harm to the planet and undermining the health and well-being of both people and the environment. This project is an attempt to shift towards a local, regenerative economy that prioritizes the well-being of people and the planet and fosters their mutual benefit.

## Example

Let's put into an example what we're trying to achieve.

A group of local people got together and decided to create a cooperative and agreed to provide their goods and services within this cooperative. All exchanges will be recorded in the private accounting system. As a medium of exchange members of the group will be using mutual credit tokens. Tokens are not backed by fiat currency or commodity, instead, they are backed by a member's commitment to contribute a certain amount of time and skills to the community. Within the cooperative members can create rules on how to distribute, circulate, and use those credits. Those rules don't have to correlate or mimic any behaviour of the current real-world economy or free market.

### Requirements 

* Accounting system should be designed to encourage the velocity of exchanges and prevent hoarding.
* The system is interest-free.
* System should provide a pool of credit(further referred to as a community pool) from which the cooperative can fund community initiatives. As well as reward volunteers who are doing good deeds for the community.
* System will have mechanisms in place to recirculate credits back into the community pool.

### Expectations

If the system operates on the local scale, it's expected:

* Higher rates of utilisation of local services and products.
* With increased velocity there's more value created and exchanged within the cooperative.
* Positive environmental impact by reducing carbon footprint due to utilization of local goods and services.
* Community resilience. An increase in engagement within the local community will result in food security and emergency preparedness. 

## How

Here's more practical info on how this can be achieved with Hyperledger Iroha.

### Variables
Initially, there are a few variables that are set by the committee members:

`AMOUNT_MINTED_PP` - Amount of minted credit per person
`DEMURAGE_CHECK_PRECENT_TIER_$X`- Percentage of minted credit per person to check if the user qualifies for demurrage within tier $X
`DEMURAGE_FEE_PRECENT_TIER_$X` - Percentage of demurrage payment for tier $X
`MINIMUM_DONATION_AMOUNT` - Minimum limit for a recurring monthly donation

The variables above are used as control levers of the system. They are initially agreed upon and set by the cooperative, later they can be fine-tuned to ensure optimal velocity of the credits.

Calculated variables:
`TOTAL_AMOUNT_MINTED` - Total amount of minted credit in the network


### Logic
#### 1. Dynamic minting of credit.
When a new member joins the cooperative, the system will execute a smart contract that mints `MINTED_AMOUNT_PP` of credits. When credits are minted, they're sent to the distribution account.
When a person leaves the cooperative, the smart contract burns `MINTED_AMOUNT_PP` of credits from the Balance pool(referenced below).

#### 2. Redistribution
When new credits are minted, as well as when existing credits are returned to circulation through recurring donations or demurrage, these credits are sent to a distribution account. This account will redistribute credits into 4 different pools in the following proportions:

70% Community pool. The community pool is dedicated to funding community initiatives dedicated to the environment, education and social welfare.

10% Rewards pool. To reward the most active and involved members of the cooperative. The pool is capped at 10% of `TOTAL_AMOUNT_MINTED`. Excess is to be sent to the community pool.

10% Committee pool. To support committee members of the cooperative. To pay for admin work, organisation of events, incubating projects, maintenance and functioning of the cooperative.

10% Balance pool. To burn credit when the person leaves. The pool is capped at 10% of `TOTAL_AMOUNT_MINTED`. Excess is to be sent to the community pool. 

#### 3. Recurring donation
It required having mechanisms in place to introduce credits back into circulation. One of them is a recurring donation.
By joining the cooperative members agree to contribute a monthly donation, which has a minimum limit of `MINIMUM_DONATION_AMOUNT`. This payment is to be sent to the distribution account. Recurring payments will be set when members engage with the system for the first time. Later members can alter the amount of donation.
If a member lacks sufficient credit to meet the minimum contribution or set donation, any remaining credits will be automatically transferred as a donation. However, if the member's balance is at zero, no donation will be made.

#### 4. Demurrage.
To prevent hoarding and recirculate credits, mechanisms of demurrage will be implemented.
Demurrage is a fee for holding onto big amounts of credit over a long period of time. 
In this implementation, there will be 3 tiers of demurrage. Contracts for demurrage are implemented sequentially from higher to lower. Implementation of the demurrage is similar for all the tires only variables differ. Demurrage contracts are executed once a month on the set date.

If `averageMonthlyBalance`(referenced below) of user exceeds `DEMURAGE_CHECK_PRECENT_TIER_$X * AMOUNT_MINTED_PP` and if `currentBalance` grater than `DEMURAGE_CHECK_PRECENT_TIER_$X * AMOUNT_MINTED_PP`, then smart contracts executes transaction of `(currentBalance - DEMURAGE_CHECK_PRECENT_TIER_$X * AMOUNT_MINTED_PP) * DEMURAGE_FEE_PRECENT_TIER_$X` to the redistribution account.

#### 5. Monitoring
To execute demurrage contracts it requires monitoring of `averageMonthlyBalance` for each user.

To ensure the optimal efficiency of the system it is required to track the velocity of credits. It's possible by monitoring the following metrics:

* Total amount of transactions per day.
* Total transaction amount per day. (All the credits that have been sent that day)
* Average transaction amount per day.
* Total amount of donation per month.
* Total amount of demurrage per month.
* Average amount of demurrage per month per person.
* Average amount of donation per month per person.

All of the metrics above need to exclude transactions made from 4 credit pools(mentioned in the redistribution section). This is required to only consider user activities.
These metrics will help to fine-tune the variables and bring the system to its optimum.