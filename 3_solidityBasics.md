
# Solidity Basics

## Solidity Syntax and Structure

### Pragma
The `pragma` directive specifies the compiler version for the Solidity code. It helps ensure compatibility between your code and the compiler.
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
```

### Contract Declaration
A contract is a fundamental building block of Ethereum applications. It is similar to a class in object-oriented programming.
```solidity
contract SimpleStorage {
    // Contract code goes here
}
```

## Data Types

### Value Types
Value types are passed by value and include Booleans, integers, addresses, and fixed-point numbers.

#### Booleans
```solidity
bool isReady = true;
```

#### Integers
Solidity supports both signed (`int`) and unsigned (`uint`) integers.
```solidity
uint8 x = 255;
int y = -10;
```

#### Addresses
Addresses represent Ethereum addresses and can be used to store and manipulate account addresses.
```solidity
address owner = 0x1234567890123456789012345678901234567890;
```

### Reference Types
Reference types are more complex types and include arrays, structs, and mappings.

#### Arrays
```solidity
uint[] numbers;
string[] names;
```

#### Structs
Structs allow you to create custom data types.
```solidity
struct Person {
    string name;
    uint age;
}

Person public person = Person("Alice", 30);
```

#### Mappings
Mappings are key-value stores for storing data.
```solidity
mapping(address => uint) public balances;
```

## Functions

### Function Declaration
Functions are used to execute code within a contract.
```solidity
function set(uint x) public {
    // Function code goes here
}
```

### Function Modifiers
Modifiers can be used to change the behavior of functions.
```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Not the contract owner");
    _;
}

function set(uint x) public onlyOwner {
    // Function code goes here
}
```

### View and Pure Functions
View functions do not modify the state and only read data. Pure functions neither read nor modify the state.
```solidity
function get() public view returns (uint) {
    return storedData;
}

function add(uint a, uint b) public pure returns (uint) {
    return a + b;
}
```

## Events

### Defining and Emitting Events
Events allow logging of data that can be accessed through the transaction logs.
```solidity
event DataStored(uint data);

function storeData(uint data) public {
    storedData = data;
    emit DataStored(data);
}
```

## Error Handling

### Require, Assert, Revert
Solidity provides several methods for error handling.

#### Require
`require` is used to validate conditions and revert if the condition is not met.
```solidity
function transfer(address to, uint amount) public {
    require(balances[msg.sender] >= amount, "Insufficient balance");
    balances[msg.sender] -= amount;
    balances[to] += amount;
}
```

#### Assert
`assert` is used to check for conditions that should never be false. It is typically used to catch bugs.
```solidity
function increment(uint x) public pure returns (uint) {
    uint y = x + 1;
    assert(y > x);
    return y;
}
```

#### Revert
`revert` can be used to flag an error and revert the transaction with a custom error message.
```solidity
function checkValue(uint value) public pure {
    if (value < 0) {
        revert("Value must be non-negative");
    }
}
```

## Conclusion
Understanding the basics of Solidity is essential for writing efficient and secure smart contracts. Mastering data types, functions, events, and error handling will provide a solid foundation for developing decentralized applications on Ethereum.

---

For further learning:
- [Solidity Documentation](https://docs.soliditylang.org/)
- [Ethereum Development Tutorials](https://ethereum.org/en/developers/tutorials/)
