### Proof of stake

The Proof of Stake consensus algorithm was introduced back in 2011 on the Bitcointalk forum to solve the problems of the current most popular algorithm in use - Proof of Work. While they both share the same goal of reaching consensus in the blockchain, the process to reach the goal is quite different.

The Proof Of Stake algorithm uses a pseudo-random election process to select a node to be the validator of the next block, based on a combination of factors that could include the staking age, randomization, and the node’s wealth.

It’s good to note that in Proof of Stake systems, blocks are said to be ‘forged’ rather than mined. Cryptocurrencies using Proof of Stake often start by selling pre-mined coins or they launch with the Proof of Work algorithm and later switch over to Proof of Stake.

Where in Proof of Work-based systems more and more cryptocurrency is created as rewards for miners, the Proof-of-Stake system usually uses transaction fees as a reward.

Users who want to participate in the forging process, are required to lock a certain amount of coins into the network as their stake. The size of the stake determines the chances for a node to be selected as the next validator to forge the next block - the bigger the stake, the bigger the chances. In order for the process not to favor only the wealthiest nodes in the network, more unique methods are added into the selection process. The two most commonly used methods are ‘Randomized Block Selection’ and ‘Coin Age Selection’.

In the Randomized Block Selection method the validators are selected by looking for nodes with a combination of the lowest hash value and the highest stake and since the size of stakes are public, the next forger can usually be predicted by other nodes.

The Coin Age Selection method chooses nodes based on how long their tokens have been staked for. Coin age is calculated by multiplying the number of days the coins have been held as stake by the number of coins that are staked. Once a node has forged a block, their coin age is reset to zero and they must wait a certain period of time to be able to forge another block - this prevents large stake nodes from dominating the blockchain.

Each cryptocurrency using Proof of Stake algorithm has their own set of rules and methods combined for what they think is the best possible combination for them and their users.

When a node gets chosen to forge the next block, it will check if the transactions in the block are valid, signs the block and adds it to the blockchain. As a reward, the node receives the transaction fees that are associated with the transactions in the block.

If a node wants to stop being a forger, its stake along with the earned rewards will be released after a certain period of time, giving the network time to verify that there are no fraudulent blocks added to the blockchain by the node.



Security
The stake works as a financial motivator for the forger node not to validate or create fraudulent transactions. If the network detects a fraudulent transaction, the forger node will lose a part of its stake and its right to participate as a forger in the future. So as long as the stake is higher than the reward, the validator would lose more coins than it would gain in case of attempting fraud.

In order to effectively control the network and approve fraudulent transactions, a node would have to own a majority stake in the network, also known as the 51% attack. Depending on the value of a cryptocurrency, this would be very impractical as in order to gain control of the network you would need to acquire 51% of the circulating supply.

The main advantages of the Proof of Stake algorithm are energy efficiency and security.
A greater number of users are encouraged to run nodes since it’s easy and affordable. This along with the randomization process also makes the network more decentralized, since mining pools are no longer needed to mine the blocks. And since there is less of a need to release many new coins for a reward, this helps the price of a particular coin stay more stable.

It’s good to remember that the cryptocurrency industry is rapidly changing and evolving and there are also several other algorithms and methods being developed and experimented with.


### Byzantine Fault Tolerance

In a few words, the Byzantine Generals’ Problem was conceived in 1982 as a logical dilemma that illustrates how a group of Byzantine generals may have communication problems when trying to agree on their next move.

The dilemma assumes that each general has its own army and that each group is situated in different locations around the city they intend to attack. The generals need to agree on either attacking or retreating. It does not matter whether they attack or retreat, as long as all generals reach consensus, i.e., agree on a common decision in order to execute it in coordination.

Therefore, we may consider the following requirements:

- Each general has to decide: attack or retreat (yes or no);
- After the decision is made, it cannot be changed;
- All generals have to agree on the same decision and execute it in a synchronized manner.
The aforementioned communication problems are related to the fact that one general is only able to communicate with another through messages, which are forwarded by a courier. Consequently, the central challenge of the Byzantine Generals’ Problem is that the messages can get somehow delayed, destroyed or lost.

In addition, even if a message is successfully delivered, one or more generals may choose (for whatever reason) to act maliciously and send a fraudulent message to confuse the other generals, leading to a total failure.

If we apply the dilemma to the context of blockchains, each general represents a network node, and the nodes need to reach consensus on the current state of the system. Putting in another way, the majority of participants within a distributed network have to agree and execute the same action in order to avoid complete failure.

Therefore, the only way to achieve consensus in these types of distributed system is by having at least ⅔ or more reliable and honest network nodes. This means that if the majority of the network decides to act maliciously, the system is susceptible to failures and attacks (such as the 51% attack).

Byzantine Fault Tolerance (BFT)

In a few words, Byzantine fault tolerance (BFT) is the property of a system that is able to resist the class of failures derived from the Byzantine Generals’ Problem. This means that a BFT system is able to continue operating even if some of the nodes fail or act maliciously. 

There is more than one possible solution to the Byzantine Generals’ Problem and, therefore, multiple ways of building a BFT system. Likewise, there are different approaches for a blockchain to achieve Byzantine fault tolerance and this leads us to the so-called consensus algorithms.

Blockchain consensus algorithms
We can define a consensus algorithm as the mechanism through which a blockchain network reach consensus. The most common implementations are Proof of Work (PoW) and Proof of Stake (PoS). But let’s take the Bitcoin case as an example.

While the Bitcoin protocol prescribes the primary rules of the system, the PoW consensus algorithm is what defines how these rules will be followed in order to reach consensus (for instance, during the verification and validation of transactions).

Although the concept of Proof of Work is older than cryptocurrencies, Satoshi Nakamoto developed a modified version of it as an algorithm that enabled the creation of Bitcoin as a BFT system.

Note that the PoW algorithm is not 100% tolerant to the Byzantine faults, but due to the cost-intensive mining process and the underlying cryptographic techniques, PoW has proven to be one of the most secure and reliable implementations for blockchain networks. In that sense, the Proof of Work consensus algorithm, designed by Satoshi Nakamoto, is considered by many as one of the most genius solutions to the Byzantine faults.