#Project Title
# SalePurchase Smart Contract

# Description
The SalePurchase smart contract facilitates the sale and purchase of an item between a seller and a buyer on the Ethereum blockchain. The contract ensures that the item can only be purchased once and provides functions for the seller to cancel the sale and for the buyer to confirm receipt.

# Features

- Set up a sale with item details including name and price.
- Purchase the item by sending the correct amount of Ether.
- Confirm receipt of the item by the buyer.
- Cancel the sale if the item has not been sold yet.
- Check the current state of the sale for consistency.
  # Code
  // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract SalePurchase {
    address public seller;
    address public buyer;
    string public itemName;
    uint public itemPrice;
    bool public isSold;

    modifier onlySeller() {
        require(msg.sender == seller, "Only the seller can perform this action");
        _;
    }

    modifier onlyBuyer() {
        require(msg.sender == buyer, "Only the buyer can perform this action");
        _;
    }

    constructor(string memory _itemName, uint _itemPrice) {
        seller = msg.sender;
        itemName = _itemName;
        itemPrice = _itemPrice;
        isSold = false;
    }

    function purchase() public payable {
        require(!isSold, "Item has already been sold");
        require(msg.value == itemPrice, "Incorrect price sent");

        buyer = msg.sender;
        isSold = true;

        payable(seller).transfer(msg.value);
    }

    function confirmReceipt() public view onlyBuyer {
        // Additional logic for confirming receipt can be added here

        // Using assert to check invariants
        assert(isSold == true);
        assert(address(this).balance == 0);
    }

    function cancelSale() public onlySeller {
        require(isSold, "Cannot cancel sale, item already sold");

        // Reset sale details
        buyer = address(0);
        isSold = false;
    }

    function checkState() public view {
        // Using assert to check for consistency
        assert(seller != address(0));
        assert(itemPrice > 0);
        assert(isSold == (buyer != address(0)));
    }
}
# Authors

Atul Mishra
- [@atulmishra4709](https://github.com/atulmishra4709)

# License
This project is licensed under the MIT License - see the LICENSE file for details.
