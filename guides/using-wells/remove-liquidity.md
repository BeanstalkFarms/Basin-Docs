# Remove Liquidity

1. Make sure you are on [basin.exchange](https://basin.exchange/) and [connect your wallet](../basics/connect-wallet.md).
2. Navigate to the “Liquidity” page. Under the "My Liquidity Positions" tab, select the Well from which you want to remove liquidity.
3. Select "Add/Rm Liquidity" then "Remove Liquidity".
4. Input the quantity of LP tokens you would like to remove.
   * You can receive one token in the Well by selecting "Single Token" or both tokens by selecting "Multiple Tokens".
   * If you select "Multiple Tokens", selecting "Claim in balanced proportion" will return an equal dollar value of each token.
   * If you select "Single Token", or "Multiple Tokens" and do not "Claim in balanced proportion", your transaction will have a price impact on the Well.
5. A transaction preview including price impact can be viewed by expanding the "Expected Output" toggle.
6. You may select a slippage tolerance by selecting the gear icon under "Expected Output". The default slippage tolerance is 0.1%.
7. If you have previously approved the LP tokens, skip to Step 9. Otherwise, select “Approve \[Token]”. This allows the Basin contract to spend the LP token, but does not remove liquidity yet.
8. Confirm the approval transaction in your wallet, and your hardware wallet, if applicable. You should verify that the transaction is interacting with the [correct contract](../../resources/contracts.md) before signing it.
9. Select “Remove Liquidity”.
10. Confirm the transaction in your wallet and your hardware wallet, if applicable. You should verify that the transaction is interacting with the [correct contract](../../resources/contracts.md) before signing it.
11. After the transaction has been confirmed by the network, your tokens will be in your wallet.
    
