# Add Liquidity

1. Make sure you are on [basin.exchange](https://basin.exchange/) and [connect your wallet](../basics/connect-wallet.md).
2. Navigate to the “Liquidity” page. The "View Wells" tab shows available Wells. Currently, you can add liquidity to the BEAN/WETH Well.
3. Select the Well and select "Add/Rm Liquidity".
4. Under "Add Liqudity", input the quantity of each token you want to add in exchange for LP tokens.
   * If you have both tokens, you can select "Add tokens in balanced proportion" to add an equal dollar value of each token.
   * If you are adding only one token, your transaction will have a price impact on the Well.
5. A transaction preview including price impact can be viewed by expanding the "Expected Output" toggle.
6. You may select a slippage tolerance by selecting the gear icon under "Expected Output". The default slippage tolerance is 0.1%.
7. If you are Depositing ETH or have previously approved the asset being spent, skip to Step 10. For all other assets, select “Approve \[Token]”. This allows the Beanstalk contract to spend the asset, but does not Deposit it yet.
8. Confirm the approval transaction in your wallet, and your hardware wallet, if applicable. You should verify that the transaction is interacting with the [correct contract](../../resources/contracts.md) before signing it.
9. Select “Add Liquidty”.
10. Confirm the transaction in your wallet and your hardware wallet, if applicable. You should verify that the transaction is interacting with the [correct contract](../../resources/contracts.md) before signing it.
11. After the transaction has been confirmed by the network, view your liquidity by navigateing to the "Liquidity" page and selecting "My Liquidity Positions".
