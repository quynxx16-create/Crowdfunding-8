# Crowdfunding-8
Crowdfunding.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Crowdfunding {
    address public owner;
    uint256 public goal;
    uint256 public deadline;
    uint256 public totalRaised;

    mapping(address => uint256) public contributions;

    constructor(uint256 _goal, uint256 _durationDays) {
        owner = msg.sender;
        goal = _goal;
        deadline = block.timestamp + _durationDays * 1 days;
    }

    function contribute() public payable {
        require(block.timestamp < deadline, "Campaign ended");
        contributions[msg.sender] += msg.value;
        totalRaised += msg.value;
    }

    function withdraw() public {
        require(msg.sender == owner, "Only owner");
        require(totalRaised >= goal, "Goal not reached");
        payable(owner).transfer(address(this).balance);
    }
}
