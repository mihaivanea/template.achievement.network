# Supply Chain Management exercise

  Smart contracts can be used to manage the supply chain of a product and track its history as it moves through different stages of production.
  As the product moves through different stages, the identities of the suppliers/producers can be recorded. This creates a production 'history' and serves as a guarantee of the quality and origin on that particular good. Consumers can themselves check if the seller of that product follows all the regulations they are subject to. Once the product went through all the stages a buyer can pay for it, meaning that each supplier gets paid the cost at each production stage for making that good.

{% exercise %}
Fill in the code to complete the functionality of the contract.

{% initial %}
pragma solidity ^0.4.24;

contract SupplyChain {
    
    // number of stages of production a good needs to go through before it can be sold - arbitrary
    uint private numberStages = 4;
    
    // as the good goes through different stages of production, their addresses are stored here in order
    address[] public stages;
    
    // for each stage, the costs of production of each supplier is added in costs
    // Add a data structure to store <customer address, cost> pairs bellow
    //
     
    modifier productCodeCheck(bytes32 code) {
        require(code == "FU6FI28B8N2H");
        _;
    }
    
    // function that checks if the good has gone through all the required stages
    function isFinalisedProduct() public view returns(bool) {
        return numberStages == stages.length;
    }
    
    // function which records who performs a new stage of production and records how much it costs
    function goThroughStage(bytes32 code) public payable productCodeCheck(code) {
        // Add code bellow to get the address of the process running the stage

        address newStage = 
        uint stageCost = msg.value;
        stages.push(newStage);
        costs[newStage] = stageCost;
    }
    
    function getCurrentStage(bytes32 code) public view productCodeCheck(code) returns(address) {
        return stages[stages.length - 1];
    }
    
    function buyGood(bytes32 code) public payable productCodeCheck(code) {
        // Add code to pay the seller

        require(price >= a.balance && isFinalisedProduct());
        a.transfer(price);
        paySupliers();
    }
    
    function paySupliers() public payable {
        for (uint i = 0; i < numberStages; i++) {
            // Add code to iterate thorugh the stages and pay each supplier

        }
    }

{% solution %}
pragma solidity ^0.4.24;

contract SupplyChain {
    
    // number of stages of production a good needs to go through before it can be sold - arbitrary
    uint private numberStages = 4;
    
    // as the Good goes through different stages of production, their addresses are stored here in order
    address[] public stages;
    
    // for each stage, the costs of production of each supplier is added in costs
    mapping(address => uint) public costs;
    
    // 
    modifier productCodeCheck(bytes32 code) {
        require(code == "FU6FI28B8N2H");
        _;
    }
    
    // function that checks if the good has gone through all the required stages
    function isFinalisedProduct() public view returns(bool) {
        return numberStages == stages.length;
    }
    
    // function which records who performs a new stage of production and how much it costs
    function goThroughStage(bytes32 code) public payable productCodeCheck(code) {
        address newStage = msg.sender;
        uint stageCost = msg.value;
        stages.push(newStage);
        costs[newStage] = stageCost;
        address a = address(this);
    }
    
    function getCurrentStage(bytes32 code) public view productCodeCheck(code) returns(address) {
        return stages[stages.length - 1];
    }
    
    function buyGood(bytes32 code) public payable productCodeCheck(code) {
        address buyer = msg.sender;
        uint price = msg.value;
        address a = address(this);
        require(price >= a.balance && isFinalisedProduct());
        a.transfer(price);
        paySupliers();
    }
    
    function paySupliers() public payable {
        for (uint i = 0; i < numberStages; i++) {
            address supplier = stages[i];
            uint cost = costs[supplier];
            supplier.transfer(cost);
        }
    }

{% validation %}
pragma solidity ^0.4.24;

import 'Assert.sol';
import 'SupplyChain.sol';

contract TestSupplyChain {
    SupplyChain public sc = new SupplyChain();
    bytes32 public productCode = "FU6FI28B8N2H";
    function testSupplyChainHistory() public {
        sc.goThroughStage(productCode);
        sc.goThroughStage(productCode);
        sc.goThroughStage(productCode);
        Assert.equal(sc.isFinalisedProduct(), false, "The product did not go through all stages");
        sc.goThroughStage(productCode);
        Assert.equal(sc.isFinalisedProduct(), true, "The product is finalised");
    }
}

{% endexercise %}

This exercise introduced you to one application of smart contracts. There are plenty more to try out! 