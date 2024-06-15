# Revised
# YapDegenTokenV2 Smart Contract

## Overview

YapDegenTokenV2 is an ERC20-compatible token with additional features for in-game store management and item redemption. This smart contract allows the owner to mint and manage tokens, and users to redeem tokens for items in predefined locations within the game.

## Features

- **Minting Tokens**: Only the owner can mint new tokens.
- **Redeeming Tokens**: Users can redeem tokens for items available in the in-game store at specific locations.
- **Burning Tokens**: Any user can burn their own tokens.
- **Checking token balance**: Users should be able to check their token balance at any time.|
- **Transferring tokens**: Users should be able to transfer their tokens to others
- **In-Game Store Management**: The owner can add, remove, update prices, and update quantities of items in the in-game store.
- **Tracking Redeemed Items**: Keeps track of items redeemed by users and their respective locations.

## Contract Details

### Prerequisites

- **Solidity Version**: 0.8.0
- **OpenZeppelin Contracts**: ERC20 and Ownable

### Contract Structure

- **Item**: Represents an item and its location.
- **StoreItem**: Represents store item details, including price and quantity.
- **Predefined Locations**:
  - ENCHANTED_FOREST
  - DRAGON_LAIR
  - CRYSTAL_CAVE

### Events

- **Minted**: Logs when new tokens are minted.
- **Redeemed**: Logs when tokens are redeemed for an item.
- **Burned**: Logs when tokens are burned.
- **ItemAddedToStore**: Logs when a new item is added to the store.
- **ItemRemovedFromStore**: Logs when an item is removed from the store.
- **ItemPriceUpdated**: Logs when an item's price is updated.
- **ItemQuantityUpdated**: Logs when an item's quantity is updated.

## Functions

### Constructor

Initializes the contract with some predefined items in the store:

```solidity
constructor() ERC20("Degen", "DGN") {
    addItemToStore("Mystic Wand", 120, 10);
    addItemToStore("Warrior's Shield", 180, 5);
    addItemToStore("Healing Elixir", 60, 20);
}
```

### Minting

Allows the owner to mint new tokens:

```solidity
function mint(address to, uint256 amount) public onlyOwner {
    _mint(to, amount);
    emit Minted(to, amount);
}
```

### Redeeming

Allows users to redeem tokens for items in the in-game store:

```solidity
function redeem(string memory itemName, string memory location, uint256 quantity) public {
    require(validLocation(location), "Invalid location");

    StoreItem storage item = storeItems[itemName];
    uint256 totalPrice = item.price * quantity;
    require(itemAvailable(item, quantity), "Item not available or insufficient quantity");
    require(balanceOf(msg.sender) >= totalPrice, "Insufficient balance");

    _transfer(msg.sender, owner(), totalPrice);
    _burn(msg.sender, totalPrice);
    item.quantity -= quantity;

    redeemedItems[msg.sender].push(Item(itemName, location, quantity));
    emit Redeemed(msg.sender, totalPrice, itemName, location);
}

function validLocation(string memory location) internal view returns (bool) {
    return keccak256(bytes(location)) == keccak256(bytes(ENCHANTED_FOREST)) ||
           keccak256(bytes(location)) == keccak256(bytes(DRAGON_LAIR)) ||
           keccak256(bytes(location)) == keccak256(bytes(CRYSTAL_CAVE));
}

function itemAvailable(StoreItem storage item, uint256 quantity) internal view returns (bool) {
    return item.price > 0 && item.quantity >= quantity;
}
```

### Burning

Allows any user to burn their own tokens:

```solidity
function burn(uint256 amount) public {
    _burn(msg.sender, amount);
    emit Burned(msg.sender, amount);
}
```

### Store Management

Owner-only functions to manage the in-game store:

- **Add Item**:

    ```solidity
    function addItemToStore(string memory itemName, uint256 price, uint256 quantity) public onlyOwner {
        storeItems[itemName] = StoreItem(price, quantity);
        emit ItemAddedToStore(itemName, price, quantity);
    }
    ```

- **Remove Item**:

    ```solidity
    function removeItemFromStore(string memory itemName) public onlyOwner {
        require(storeItems[itemName].price > 0, "Item not found in store");
        delete storeItems[itemName];
        emit ItemRemovedFromStore(itemName);
    }
    ```

- **Update Item Price**:

    ```solidity
    function updateItemPrice(string memory itemName, uint256 newPrice) public onlyOwner {
        require(storeItems[itemName].price > 0, "Item not found in store");
        storeItems[itemName].price = newPrice;
        emit ItemPriceUpdated(itemName, newPrice);
    }
    ```

- **Update Item Quantity**:

    ```solidity
    function updateItemQuantity(string memory itemName, uint256 newQuantity) public onlyOwner {
        require(storeItems[itemName].price > 0, "Item not found in store");
        storeItems[itemName].quantity = newQuantity;
        emit ItemQuantityUpdated(itemName, newQuantity);
    }
    ```

### View Functions

- **Get Store Item Price**:

    ```solidity
    function getStoreItemPrice(string memory itemName) public view returns (uint256) {
        return storeItems[itemName].price;
    }
    ```

- **Get Store Item Quantity**:

    ```solidity
    function getStoreItemQuantity(string memory itemName) public view returns (uint256) {
        return storeItems[itemName].quantity;
    }
    ```

- **Get Redeemed Items for a User**:

    ```solidity
    function getRedeemedItems(address user) public view returns (Item[] memory) {
        return redeemedItems[user];
    }
    ```

## Usage

1. **Minting Tokens**: Only the owner can mint new tokens using the `mint` function.
2. **Redeeming Items**: Users can redeem their tokens for items using the `redeem` function, specifying the item name, location, and quantity.
3. **Burning Tokens**: Any user can burn their own tokens using the `burn` function.
4. **Managing the Store**: The owner can add, remove, update prices, and update quantities of items in the store using the respective functions.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
