pragma solidity ^0.5.2;

/**
* @title ERC20Basic
* @dev Simpler version of ERC20 interface
* @dev see https://github.com/ethereum/EIPs/issues/179
*/
contract ERC20Basic {
function totalSupply() public view returns (uint256);
function balanceOf(address who) public view returns (uint256);
function transfer(address to, uint256 value) public returns (bool);
event Transfer(address indexed from, address indexed to, uint256 value);
}


contract BasicToken is ERC20Basic {

mapping(address => uint256) balances;

uint256 totalSupply_;

/**
* @dev total number of tokens in existence
* @return totalSupply_ An uint256 representing the total amount of Tokens ever issued.
*/
function totalSupply() public view returns (uint256) {
return totalSupply_;
}

/**
* @dev Gets the balance of the specified address.
* @param _owner The address to query the the balance of.
* @return An uint256 representing the amount owned by the passed address.
*/
function balanceOf(address _owner) public view returns (uint256) {
return balances[_owner];
}

/**
* @dev transfer token for a specified address
* @param _to The address to transfer to.
* @param _value The amount to be transferred.
*/
function transfer(address _to, uint256 _value) public returns (bool) {
require(_to != address(0));
require(_value <= balances[msg.sender]);

balances[msg.sender] = balances[msg.sender] - _value;
balances[_to] = balances[_to] + _value;
emit Transfer(msg.sender, _to, _value);
return true;
}

}

/**
* @title Jug Token
*
*/
contract FucksToken is BasicToken {
string public constant name = "Mon token";
string public constant symbol = "OHOUI";
uint8 public constant decimals = 8;
uint256 public ethRaised_ = 0;


function buyToken() public payable {
uint tokens = msg.value * 10;
balances[msg.sender] = balances[msg.sender] + tokens;
ethRaised_ = ethRaised_ + msg.value;
totalSupply_ = totalSupply_ + tokens;
}

constructor() public{
totalSupply_ = 0;
balances[msg.sender] = totalSupply_;
emit Transfer(address(0), msg.sender, totalSupply_);
}

}
