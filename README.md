# Profit_Splitter

Smart contract that is designed to distribute company shares automatically.

1 . The Associate Profit Splitter will distribute shares evenly. 

pragma solidity ^0.5.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC721/ERC721Full.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/drafts/Counters.sol";

// 1: Equal Split
contract AssociateProfitSplitter {
    
    address payable employee_one;
    address payable employee_two;
    address payable employee_three;
   
    constructor(address payable _one, address payable _two, address payable _three) public {
        employee_one = _one;
        employee_two = _two;
        employee_three = _three;
    }

    function balance() public view returns(uint) {
        return address(this).balance;
    }

    function deposit() public payable {
        
        uint amount = msg.value / 3; 
            employee_one.transfer(amount);
            employee_two.transfer(amount);
            employee_three.transfer(amount);
        
        msg.sender.transfer(msg.value - amount * 3);
        
    }

    function() external payable {
    }
}

chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/home.html#confirm-transaction/7715734418935019/deploy-contract
![image](https://user-images.githubusercontent.com/8082238/133845489-afd68e9f-a685-463b-b7d7-dc6b5f8d2aed.png)


2 . The Tiered Profit Splitter will distrubte the shares according to the employee tier or level. The CEO will receive 60%, CTO 25%, and lower level will receive 15%.

pragma solidity ^0.5.0;

// lvl 2: tiered split
contract TieredProfitSplitter {
    address payable employee_one; // ceo
    address payable employee_two; // cto
    address payable employee_three; // bob

    constructor(address payable _one, address payable _two, address payable _three) public {
        employee_one = _one;
        employee_two = _two;
        employee_three = _three;
    }

    // Should always return 0! Use this to test your `deposit` function's logic
    function balance() public view returns(uint) {
        return address(this).balance;
    }

    function deposit() public payable {
        uint points = msg.value / 100; // Calculates rudimentary percentage by dividing msg.value into 100 units
        uint total;
        uint amount;

        
        // Step 1: Set amount to equal `points` * the number of percentage points for this employee
        amount = points * 60;
        // Step 2: Add the `amount` to `total` to keep a running total
        total += amount;
        // Step 3: Transfer the `amount` to the employee
        employee_one.transfer(amount);

        // @TODO: Repeat the previous steps for `employee_two` and `employee_three`
        amount = points * 25;
        total += amount;
        employee_two.transfer(amount);
        
        amount = points * 15;
        total += amount;
        employee_three.transfer(amount);

        employee_one.transfer(msg.value - total); // ceo gets the remaining wei
    }

    function() external payable {
        deposit();
    }
}

chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/home.html#confirm-transaction/7715734418935020/deploy-contract
![image](https://user-images.githubusercontent.com/8082238/133845557-83fc5df7-4708-43b9-ad8a-1ebdb1ee3f96.png)
