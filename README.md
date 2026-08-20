# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:
Step 1: Setup Lending and Borrowing Mechanism
Users deposit ETH into the contract as liquidity.


Depositors receive interest based on their deposits.


Borrowers can borrow ETH but must provide collateral (e.g., 150% of the borrowed amount).


Interest on borrowed funds is calculated dynamically based on utilization rate.


Step 2: Implement Overcollateralization
If a borrower’s collateral value drops below a certain liquidation threshold, their collateral is liquidated to repay the debt.


Step 3: Allow Liquidation
If collateral < liquidation threshold, liquidators can repay the borrower's debt and claim their collateral at a discount.



Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {

    uint256 public interestRate = 20;   // 20% interest per cycle
    uint256 public liquidationThreshold = 150; 

    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Borrowed(address user, uint256 amount, uint256 collateral);
    event InterestAdded(address user, uint256 newDebt);
    event Liquidated(address user, uint256 collateralSeized);

    // Borrow function (Collateral must be given here)
    function borrow(uint256 amount) public payable {

        require(msg.value >= (amount * liquidationThreshold)/100, 
        "Not enough collateral");

        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;

        payable(msg.sender).transfer(amount);

        emit Borrowed(msg.sender, amount, msg.value);
    }

    // Interest added to increase debt
    function addInterest() public {

        uint256 interest = (borrowed[msg.sender] * interestRate)/100;
        borrowed[msg.sender] += interest;

        emit InterestAdded(msg.sender, borrowed[msg.sender]);
    }

    // Liquidation
    function liquidate(address borrower) public {

        require(
        collateral[borrower] < (borrowed[borrower] * liquidationThreshold)/100,
        "Not eligible for liquidation"
        );

        uint seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;

        payable(msg.sender).transfer(seizedCollateral);

        emit Liquidated(borrower, seizedCollateral);
    }

    // Deposit ETH into contract so it can lend
    receive() external payable {}
}

```
# Expected Output:
<img width="1920" height="1080" alt="Screenshot 2026-08-20 140042" src="https://github.com/user-attachments/assets/08790340-44d7-4e48-869c-e5b5ff00e86d" />



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
Thus, the DeFi Lending and Borrowing smart contract was successfully deployed, demonstrating borrowing, collateralization, interest accrual, and liquidation mechanisms in a decentralized finance system.

