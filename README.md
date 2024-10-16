# OtcomToken (OTOM) - Smart Contract

## Overview
**OtcomToken** is a decentralized ERC20-compliant token built on Ethereum, designed for automated liquidity management and developer fund allocation. It incorporates features such as token burning, blacklist management, taxation on transactions, and trade control, leveraging Uniswap for liquidity management.

## Table of Contents
- [Token Details](#token-details)
- [Features](#features)
- [Events](#events)
- [Functions](#functions)
  - [Token Information](#token-information)
  - [Transactions](#transactions)
  - [Trade and Tax Management](#trade-and-tax-management)
  - [Liquidity and Dev Tax](#liquidity-and-dev-tax)
  - [Burning Tokens](#burning-tokens)
- [Taxation Logic](#taxation-logic)
- [Blacklisting](#blacklisting)
- [Deployment and Configuration](#deployment-and-configuration)
- [Receiving and Sending ETH](#receiving-and-sending-eth)
- [Security Considerations](#security-considerations)
- [License](#license)

## Token Details
- **Name**: OtcomToken
- **Symbol**: OTOM
- **Decimals**: 18
- **Total Supply**: 10,000,000 OTOM
- **Max Buy/Sell Limit**: 20,000 OTOM (adjustable)

## Features
- **Liquidity Provision**: Automatically adds liquidity when the contract reaches a threshold balance.
- **Developer Tax**: A portion of each transaction is sent to a developer wallet.
- **Token Burning**: The owner can burn tokens to reduce the total supply.
- **Blacklist Management**: Certain addresses can be blacklisted, preventing them from participating in token transactions.
- **Trading Control**: The owner can enable trading at will.
- **Taxation**: Buy and sell transactions are subject to liquidity and developer taxes.

## Events
- `UpdatedDevWallet(address updatedDevWallet)`  
  Emitted when the developer wallet is updated.
  
- `UpdatedTaxPercentage(uint256 updatedLiquidityTax, uint256 updatedDevTax)`  
  Emitted when the liquidity and developer tax percentages are updated.
  
- `UpdatedTaxThreshold(uint256 updateTaxThreshold)`  
  Emitted when the tax threshold is updated.
  
- `UpdatedMaxAmount(uint256 updatedMaxAmount)`  
  Emitted when the maximum transaction amount is updated.
  
- `UpdatedBlock(uint256 updatedBlocks)`  
  Emitted when the number of blocks for blacklist management is updated.
  
- `Burn(address indexed burner, uint256 amount)`  
  Emitted when tokens are burned.

## Functions

### Token Information
- **`name()`**  
  Returns the name of the token.

- **`symbol()`**  
  Returns the symbol of the token.

- **`decimals()`**  
  Returns the number of decimals used by the token.

- **`totalSupply()`**  
  Returns the total supply of the token.

- **`balanceOf(address account)`**  
  Returns the token balance of a specific account.


### Transactions
- **`transfer(address to, uint256 amount)`**  
  Transfers tokens to the specified address.

- **`transferFrom(address from, address to, uint256 amount)`**  
  Transfers tokens from one account to another.

- **`approve(address spender, uint256 amount)`**  
  Approves a spender to spend a specific amount of tokens on behalf of the caller.

- **`allowance(address owner, address spender)`**  
  Returns the amount a spender is allowed to spend on behalf of an owner.

### Trade and Tax Management
- **`enableTrade()`**  
  Enables  trading.

- **`setNumberOfBlocksForBlacklist(uint256 numBlocks)`**  
  Sets the number of blocks after launch for blacklist management.
  
     ` numBlocksForBlacklist(curenntly set 50 block after you can change)`
    - numBlocksForBlacklist sets the number of blocks during which anyone who interacts with the contract will be blacklisted. Specifically, after the liquidity pool (Uniswap pair) is created, the contract starts counting blocks from the       current block number (currentBlockNumber). If a recipient interacts with the contract within the first numBlocksForBlacklist blocks, they are automatically blacklisted, meaning they cannot trade or transfer the token anymore.
    - Purpose: This is typically used as an anti-bot mechanism to prevent sniper bots or malicious actors from taking advantage of the token launch by buying tokens in the first few blocks after trading is enabled. By setting a block           limit, the contract can automatically blacklist such addresses, preventing them from making further trades.

- **`setMaxAmount(uint256 amount)`**  
  The setMaxAmount function allows the contract owner to set a maximum limit for the amount of tokens that can be transferred in a single transaction. However, the new limit cannot exceed 100,000 tokens (with 18 decimal places).

- **`setDevWallet(address wallet)`**  
  Updates the developer wallet address.

- **`setTaxPercentage(uint256 _taxPercentage)`**  
  Updates the liquidity and developer tax percentages (max 25%).

- **`setTaxThreshold(uint256 _threshold)`**  
  Sets the threshold at which the contract will perform swap and liquify.

### Liquidity and Dev Tax
- **`swapTokensForEth(uint256 tokenAmount)`**  
  Swaps the token amount for ETH using Uniswap.

- **`swapAndLiquify()`**  
  Automatically swaps tokens and adds liquidity to Uniswap.

- **`addLiquidity(uint256 tokenAmount, uint256 ethAmount)`**  
  Adds liquidity to Uniswap.

### Burning Tokens
- **`burn(uint256 amount)`**  
  Burns the specified amount of tokens, Token does not decrease the total supply.

## Taxation Logic
The buy and sell transactions are subject to taxation:
- **Liquidity Tax**: A portion of each transaction is collected for liquidity provision.
- **Developer Tax**: A portion is sent to the developer wallet.

### Tax Calculation
- **`liquidityTaxPercentage`**: Default is 1% (1000 units, where 1000 = 1%).
- **`devTaxPercentage`**: Default is 1% (1000 units, where 1000 = 1%).

## Blacklisting
Addresses can be blacklisted within the first `numBlocksForBlacklist` blocks after launch, preventing them from participating in trades.

## Deployment and Configuration
- **Constructor Parameters**:  
  - `_devWallet`: The developer's wallet address (must be a non-zero address).
  
- **Uniswap Router Address**:  
  `0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D` (Ethereum mainnet router).

## Receiving and Sending ETH
-  **`receive()`**  
  The contract can accept ETH sent directly to it.

## Security Considerations
- Ensure the developer wallet is set correctly before enabling trading.
- Use the blacklist feature to mitigate bot attacks shortly after launch.
- Carefully manage tax percentages to maintain contract balance.
  
## Key steps:

- Add liquidity first: Liquidity will be added before enabling trading.
- Enable trading: Once liquidity is added, trading will be enabled.
- Blacklist users buying within the first 50 blocks(you can change this block): After trading is enabled, for the first 50 blocks, anyone buying from the Uniswap pool will be blacklisted and permanently restricted from trading.


## License
MIT License
