# YapDegenTokenV2

## Overview

YapDegenTokenV2 is an ERC20 token contract implemented on the Ethereum blockchain. It includes functionalities for minting, burning, and redeeming tokens for in-game items from a store. This contract also allows the owner to manage store items dynamically.

## Features

- Minting: The owner can mint new tokens.
- Burning: Users can burn their own tokens.
- Redeeming: Users can redeem tokens for in-game items from the store.
- Check Balance: Users can check the balance of a specific address
- Transfering: Can transfer tokens to a specific address. 
- Store Management: The owner can add or remove items from the in-game store.

## Dependencies

This contract uses OpenZeppelin's ERC20 and Ownable contracts for token functionality and ownership management, respectively. The contract imports these dependencies directly from the OpenZeppelin GitHub repository.

## Usage

1. **Minting Tokens**: Only the owner can mint new tokens using the `mint` function.

2. **Burning Tokens**: Users can burn their own tokens using the `burn` function.

3. **Redeeming Tokens**: Users can redeem tokens for in-game items from the store using the `redeem` function.

4. **Managing Store Items**: The owner can add new items to the store using the `addItemToStore` function and remove items from the store using the `removeItemFromStore` function.

## Events

- `Minted`: Logged when tokens are minted.
- `Redeemed`: Logged when tokens are redeemed for items.
- `Burned`: Logged when tokens are burned.
- `ItemAddedToStore`: Logged when an item is added to the store.
- `ItemRemovedFromStore`: Logged when an item is removed from the store.

## License

This contract is licensed under the MIT License. See the SPDX-License-Identifier at the beginning of the file for details.

