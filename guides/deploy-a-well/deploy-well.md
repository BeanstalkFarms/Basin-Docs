# Deploy a new Well

Basin is a composable DEX protocol, meaning that it has various modular components that can be composed together to create new liquidity pools. These components are [Well Functions](https://docs.basin.exchange/components/well#well-function), [Well implementations](https://docs.basin.exchange/components/well#well-implementation) and network-native oracles known as [Pumps](https://docs.basin.exchange/components/pump). 

There are two primary ways to deploy a new Well.

The simplest way for someone to deploy a new Well is by calling the ```boreWell``` function on the [Aquifer](https://docs.basin.exchange/components/aquifer) (factory) contract. ```boreWell``` accepts the encoded addresses of each Well component as input, along with additional parameters such as the Well's name and symbol and a salt for deterministic address generation. It then verifies that the calldata is valid and clones a pre-deployed Well template with the new parameters. In addition, a mapping of the new Well address to the existing Well Implementation address is stored in the Aquifer contract. Here how it looks like in a step by step guide:

## Step by step guide to deploy a new Well

### 1. Obtain the addresses of the components to be used in the new Well.

First, obtain the addresses of the 2 ERC-20 tokens to be traded in the new Well. You can find a number of tokens and their addresses on [Etherscan](https://etherscan.io/tokens) or any other block explorer.

Next, obtain the addresses for the desired Well Function, Well Implementation and Pump. You can find the addresses for audited and deployed Well components [here](https://docs.basin.exchange/resources/contracts).

Finally, decide on a Well name and symbol. These can be any string of your choice but it is recommended to follow the standard format.
* The standard name format is ```tokenSymbol1:tokenSymbol2 + Well Function name + "Well"```. For example assuming you are creating a well for the tokens wETH and wBTC with a Constant Product 2 function, the name would be ```wETH:wBTC Constant Product 2 well```.
* The standard symbol format is ```tokenSymbol1 + tokenSymbol2 + Well Function abbreviation + "w"```. For example assuming you are creating a Well for the tokens wETH and wBTC with the Constant Product 2 Well function, the symbol would be ```wETHwBTCCP2w```.

### 2. Encode the calldata

After getting the necessary parameters described above for the new Well, you will need to properly encode them for the Aquifer contract. There are 2 parts of calldata that need to be encoded. The first is the ```init``` function call on the Well Implementation contract, to initialize the new Well with the name and symbol. The second is the immutable data of the well, which includes the addresses of the tokens, the Pump and the Well Function.
These 2 parts should be encoded separately by using the ```abi.encodePacked``` function in Solidity or the alternative ```ethers.solidityPacked``` in JavaScript using the ```ethers``` library.

Here is an example of how to encode the data using ```ethers.js``` starting with the ```init``` function call. For the Well Implementation ABI, you can take a look at the verified Well Implementation contracts on [Etherscan](https://etherscan.io/address/0xBA510e11eEb387fad877812108a3406CA3f43a4B#code).

```javascript
// Encodes the init function call for the well implementation
async function encodeInitFunctionCall(wellImplementationAbi, wellName, wellSymbol) {
    const wellInterface = new hre.ethers.Interface(wellImplementationAbi)
                                          // function   name,  symbol     
    return wellInterface.encodeFunctionData('init', [wellName, wellSymbol]);
}

// Example usage
const wellImplementationAbi = "<ABI-FROM-ETHERSCAN>";
const wellName = "wETH:wBTC Constant Product 2 well";
const wellSymbol = "wETHwBTCCP2w";
const initFunctionCall = await encodeInitFunctionCall(wellImplementationAbi, wellName, wellSymbol);
```

Here is an example of how to encode the immutable data of the Well:

```javascript
// encodeWellImmutableData encodes the immutable data for a well
function encodeWellImmutableData(
    aquifer,
    tokens,
    wellFunction,
    pumps
  ) {
    let packedPumps = '0x';
    for (let i = 0; i < pumps.length; i++) {
        packedPumps = hre.ethers.solidityPacked(
            ['bytes', 'address', 'uint256', 'bytes'],
            [
                packedPumps,           // previously packed pumps
                pumps[i].target,       // pump address
                pumps[i].length,       // pump data length
                pumps[i].data          // pump data (bytes)
            ]
        )
    }
  
  
    immutableData = hre.ethers.solidityPacked(
        [
            'address',                  // aquifer address
            'uint256',                  // number of tokens
            'address',                  // well function address
            'uint256',                  // well function data length
            'uint256',                  // number of pumps
            'address[]',                // tokens array
            'bytes',                    // well function data (bytes)
            'bytes'                     // packed pumps (bytes)
        ], [
        aquifer,                    // aquifer address
        tokens.length,              // number of tokens
        wellFunction.target,        // well function address
        wellFunction.length,        // well function data length
        pumps.length,               // number of pumps
        tokens,                     // tokens array
        wellFunction.data,          // well function data (bytes)
        packedPumps                 // packed pumps (bytes)
    ]
    );
    return immutableData
  }

// Example usage
const aquifer = "<AQUIFER-ADDRESS>";
const wellFunctionAddress = "<WELL-FUNCTION-ADDRESS>";
const pumpAddress = "<PUMP-ADDRESS>";
const token1 = "<TOKEN1-ADDRESS>";
const token2 = "<TOKEN2-ADDRESS>";
const tokens = [token1, token2];
const wellFunction = { target: wellFunctionAddress, data: '0x', length: 0 }; // well function
const pumps = [{ target: pumpAddress, data: '0x', length: 0 }]; // pumps
const immutableData = encodeWellImmutableData(aquifer, tokens, wellFunction, pumps);
```

The final calldata from the above examples will look something like this:

```bash
Encoded Immutable Data:  0x7aa056fcef8f529e8c8e0732727f40748f49bc1b000000000000000000000000000000000000000000000000000000000000000230af8bc21683754086cdcb27c828facae85cbcad000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000003f4b6664338f23d2397c953f2ab4ce8031663f800000000000000000000000007aaaa3d35497291e85f9b3673498afc8fb8381479a3ae9143753efc2cc9583eb03defac75b6496860000000000000000000000000000000000000000000000000000000000000000

Encoded Init Data: 
0x7029144c000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000224f4b423a57535445544820436f6e7374616e742050726f6475637420322057656c6c000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000d4f4b425753544554484350327700000000000000000000000000000000000000
```


### 3. Choose a salt for deterministic address generation.
The `salt` is an additional parameter thats used to calculate the address of the deployed well. This allows it to be deployed on different EVM chains and have the same deterministic address, as long it uses the same salt. It also allows for the creation of vanity addresses by mining for a salt value that gives the wanted address. The salt is optional and can be any 32 byte value. It is recommended to use a random value for the salt to know where the new Well will be deployed beforehand.

### 4. Call the boreWell function on the Aquifer contract with the encoded data as the parameter.

You are now ready to call the ```boreWell``` function on the Aquifer contract with the encoded data as the parameter! The call would look something like this in JavaScript:

```javascript
  await deployedAquifier.boreWell(
    wellImplementationAddress,
    immutableData,
    initData,
    salt
  );
```

Congratulations! You have now deployed a new Well on Basin. The new Well will be deployed at the address returned by the ```boreWell``` function. You can now use the new Well to provide liquidity and trade tokens. 

All of the above can be achieved only assuming all the required components used in the new Well are already deployed. Thourough verification of the components is required beforehand to ensure the Well is deployed correctly. You can find the addresses for audited and deployed versions of each Well component [here](https://docs.basin.exchange/resources/contracts).
