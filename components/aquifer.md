# Aquifer

{% hint style="info" %}
See [IAquifer.sol](https://github.com/BeanstalkFarms/Basin/blob/master/src/interfaces/IAquifer.sol)
{% endhint %}

Aquifer is an instance of a Well factory (i.e., a permissionless Well deployer and registry). Aquifer deploys Wells by cloning a pre-deployed Well and stores a mapping of the new Well address to the Well Implementation address.&#x20;

Aquifer creates the first layer of trust for Basin users by providing a way to verify a Well's Implementation given its address. However, users must still verify the authenticity of the Well's Well Function, Pumps and Well Implementation.

***

{% hint style="info" %}
[Aqueduct](https://basin.exchange/basin.pdf#subsection.3.6) is a whitelist of Well Functions, Pumps, Well Implementations and Aquifers. Aqueducts create a single point of trust for users of Basin components. As long as all the components of a Well are included in a trusted Aqueduct, the Well can be trusted. In practice, protocols can deploy an Aqueduct that specifies all the Basin components liquidity providers can (or must) use in order to receive some form of credit from them.\
\
A template Aqueduct has not yet been implemented.
{% endhint %}
