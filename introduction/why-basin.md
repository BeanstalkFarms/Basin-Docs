# Why Basin

Despite the outsized importance of DEX protocols to the permissionless economy, innovation of them has been slow. The primary components of a DEX are:

1. Exchange functions, which specify the conditions under which a given asset can be exchanged for another;
2. Oracles, which save data related to the DEX relevant to other protocols that need to permissionlessly and atomically query the DEX's state; and
3. Exchange implementations, which facilitate the use (i.e., swapping, adding and removing liquidity) of the DEX.

One major problem with current DEX architectures is a lack of composability, such that to add, remove or replace a component of a DEX requires writing an entirely new DEX. This is highly inefficient.

Utility of DEXs has been limited in part because current architectures mandate market makers provide liquidity using a finite set of exchange functions that are provided by the DEX. This limits the flexibility and capital efficiency of market makers' liquidity. Lack of liquidity flexibility and capital efficiency make profitably market making on DEXs excessively difficult. Profitable market making is essential for well functioning liquid markets. **Excessive friction to customizing exchange functions to provide liquidity on DEXs makes it difficult for decentralized permissionless trading environments to compete with centralized permissioned ones in attracting market makers.** A shortage of market makers leads to a shortage of liquidity; a shortage of liquidity leads to a lack of utility for takers.

***

Saving oracle data on a decentralized network is expensive. The only reason to save DEX data in a network-native oracle is so that it can be queried permissionlessly by other network-native protocols. Current DEX architectures mandate using a particular oracle, independent of whether the data saved about the liquidity is being used by other network-native protocols. Because the cost to saving oracle data is passed on to users of the protocol, this policy imposes an often unnecessary cost on all users of the DEX.

In a post-Merge environment, the risk of [oracle manipulation due to inter-block MEV attacks](https://www.alvarorevuelta.com/posts/ethereum-mev-multiblock) has risen dramatically. This has made using existing network-native oracle solutions risky and impractical for protocols that require manipulation resistant oracles. **Upgrading the oracles in existing DEXs to be inter-block MEV resistant is impossible due to the lack of composability between exchange functions and oracles.** As a result, protocols are forced to use non-network-native oracle solutions (i.e., Chainlink) which require additional trust assumptions beyond the integrity of the network itself while users of DEXs are still paying to update useless oracles.

**Basin is an open source permissionless DEX architecture that allows for the composition of arbitrary exchange functions, network-native oracles and exchange implementations into a single liquidity pool known as a Well.** A registry of pool implementations allows for users to verify the accuracy of a pool's use of its exchange function, oracles and exchange implementation. Anyone can deploy new exchange functions, network-native oracles, exchange implementations and registries. Similarly, anyone can deploy a new pool through an existing registry with an exchange function, any (or no) network-native oracles and an exchange implementation included in the registry. In practice, Basin lowers the friction for market makers to deploy liquidity with custom orders and allow their liquidity to be used by other network-native protocols without additional trust assumptions.

***

Well designed open source protocols enable anyone to (1) compose existing components together, (2) develop new components when existing ones fail to meet the needs of users or are cost-inefficient and (3) verify the proper use of existing components in a simple fashion. Basin allows for anyone to compose new and existing (1) [**Well Functions**](../components/well.md#well-function) (i.e. exchange functions), (2) [**Pumps**](../components/pump.md) (i.e., network-native oracles) and (3) [**Well Implementations**](../components/well.md#well-implementation) (i.e., exchange implementations) to create a [**Well**](../components/well.md) (i.e., a customized liquidity pool). [**Aquifers**](../components/aquifer.md) (i.e., Well registries) store a mapping from Well addresses to Well Implementations to enable verification of a Well Implementation given a Well address.
