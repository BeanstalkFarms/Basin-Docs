# Pump

{% hint style="info" %}
See [Pump interfaces](https://github.com/BeanstalkFarms/Basin/tree/master/src/interfaces/pumps).
{% endhint %}

Pumps are a generalized framework for network-native oracles. A Pump is updated upon each interaction with a Well that uses it. A Pump can be shared by multiple Wells if it stores a mapping of Well addresses. Each Pump can determine its own method of recording a Well's Reserves.&#x20;

When there’s an interaction with a Well, the Well sends two pieces of information to its Pumps: (1) a list of Reserve amounts and (2) additional metadata as a bytestring. Each Pump is responsible for deciding how to weight the new Reserve values relative to the values it had previously and processing the bytestring.&#x20;

Because storing data on-chain is expensive, only absolutely necessary data should be recorded. Each Well can be deployed with an arbitrary set of Pumps, such that only and exactly the data necessary to satisfy composability between the liquidity in the Well and arbitrary other protocols desired by the liquidity provider is recorded on-chain. Pumps create a minimalist, composable way for liquidity providers to minimize gas costs associated with trading against their liquidity while maximizing customizability for using their liquidity in other protocols.
