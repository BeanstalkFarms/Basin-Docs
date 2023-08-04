# Well

A **Well** allows for the provisioning of liquidity into a single on-chain position that follows arbitrary rules and is represented by an ERC-20 token (i.e., a Well LP Token).&#x20;

Each Well is defined by:

1. A list of tokens, which contains the set of ERC-20 tokens that the Well supports (i.e., that can be exchanged through, added to, and removed from the Well);
2. A [**Well Function**](well.md#well-function) and its associated data, which defines an invariant relationship between the **Well's Reserves** (i.e., the balances of tokens in (1) at the time of the last supported Well operation) and the supply of Well LP tokens;
3. [**Pumps**](pump.md) and their associated data, which implement network-native oracles that are updated each time associated Well's Reserves may change;
4. A [**Well Implementation**](well.md#well-implementation), which contains all logic for actions supported by the Well;
5. Optional arbitrary additional data used by the Well; and
6. The [**Aquifer**](aquifer.md) that deployed the Well.

A Well's state consists of its Reserve balances and Well LP token ownership. Well LP tokens are ERC-20 tokens representing pro-rata ownership of the Well’s Reserves and implement [ERC-2612](https://eips.ethereum.org/EIPS/eip-2612). Well Functions and Pumps can be independently chosen to be stateful or stateless, while Well Implementations are stateful. Including Pumps in a Well is optional.

### Well Function

{% hint style="info" %}
See [IWellFunction.sol](https://github.com/BeanstalkFarms/Basin/blob/master/src/interfaces/IWellFunction.sol).
{% endhint %}

A Well Function defines an invariant relationship between a Well's Reserves and the supply of Well LP tokens. Basin supports Well Functions that contain arbitrary logic. However, Well Functions must be deterministic to be used alongside Pumps. In practice, the handling of arbitrary logic in a Well Function allows for arbitrary order creation (i.e., conditional orders that take network state data as inputs).

### Well Implementation

{% hint style="info" %}
See [IWell.sol](https://github.com/BeanstalkFarms/Basin/blob/master/src/interfaces/IWell.sol).
{% endhint %}

Well Implementations contain all logic to support swapping against, adding liquidity to and removing liquidity from a Well in arbitrary proportions. A Well Implementation may require a Well to have associated data which the implementation uses to properly interface with its Well Function (e.g., an A parameter for a [Curve](https://curve.fi/) style stableswap invariant, a trading fee, etc.) and Pumps (e.g., the look-back period of a Pump). Well Implementations can specify a whitelist (or blacklist) of acceptable (or unacceptable) counterparties and/or liquidity providers.
