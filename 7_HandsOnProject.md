### Decentralized Voting System Smart Contract

#### Step 1: Write the Smart Contracts

Create two new files: `Voting.sol` and `Ownable.sol`.

##### `Ownable.sol` (Contract for Ownership Management)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), _owner);
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Ownable: caller is not the owner");
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
```

**Explanation (`Ownable.sol`):**

- `Ownable`: This contract manages ownership, allowing only the owner (creator) to perform certain actions.
- `constructor()`: Sets the contract creator as the initial owner.
- `onlyOwner modifier`: Restricts access to functions to only the current owner.
- `transferOwnership(address newOwner)`: Allows the current owner to transfer ownership to another address.

##### `Voting.sol` (Decentralized Voting System Contract)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "./Ownable.sol";

contract Voting is Ownable {
    struct Candidate {
        uint256 id;
        string name;
        uint256 voteCount;
    }

    mapping(uint256 => Candidate) public candidates;
    mapping(address => bool) public voters;

    uint256 public candidatesCount;

    event Voted(uint256 indexed candidateId, address indexed voter);

    constructor() {
        addCandidate("Candidate 1");
        addCandidate("Candidate 2");
    }

    function addCandidate(string memory name) private onlyOwner {
        candidatesCount++;
        candidates[candidatesCount] = Candidate(candidatesCount, name, 0);
    }

    function vote(uint256 candidateId) public {
        require(candidateId > 0 && candidateId <= candidatesCount, "Invalid candidate ID");
        require(!voters[msg.sender], "Already voted");

        voters[msg.sender] = true;
        candidates[candidateId].voteCount++;

        emit Voted(candidateId, msg.sender);
    }

    function getCandidate(uint256 candidateId) public view returns (string memory name, uint256 voteCount) {
        require(candidateId > 0 && candidateId <= candidatesCount, "Invalid candidate ID");

        Candidate memory candidate = candidates[candidateId];
        return (candidate.name, candidate.voteCount);
    }
}
```

**Explanation (`Voting.sol`):**

- `Voting`: This contract implements a decentralized voting system.
- `Candidate struct`: Defines a structure to store candidate information including ID, name, and vote count.
- `candidates mapping`: Maps candidate IDs to `Candidate` structs.
- `voters mapping`: Maps addresses to a boolean indicating whether they have voted.
- `candidatesCount`: Stores the total number of candidates.
- `event Voted`: Logs voting events.
- `constructor()`: Initializes the contract by adding two candidates.
- `addCandidate(string memory name) private onlyOwner`: Function to add a new candidate. Only the contract owner can add candidates.
- `vote(uint256 candidateId) public`: Function for voters to cast their vote for a candidate.
- `getCandidate(uint256 candidateId) public view returns (string memory name, uint256 voteCount)`: Function to retrieve candidate information by ID.

#### Step 2: Compile the Smart Contracts

1. **Using Remix IDE:**
   - Go to [Remix IDE](https://remix.ethereum.org/).
   - Create two new files: `Voting.sol` and `Ownable.sol`.
   - Paste the respective Solidity code into each file.
   - Click on the "Solidity Compiler" tab.
   - Click "Compile Voting.sol" and ensure both contracts compile without errors.

#### Step 3: Deploy and Interact with the Smart Contract

1. **Deploying and Interacting in Remix:**
   - In Remix, go to the "Deploy & Run Transactions" tab.
   - Deploy the `Voting` contract. Ensure you have selected a suitable environment (e.g., JavaScript VM for testing or Injected Web3/MetaMask for deployment on a real network).
   - Interact with the contract:
     - Use the `addCandidate` function to add candidates.
     - Use the `vote` function to cast votes for candidates.
     - Use the `getCandidate` function to retrieve candidate information.

#### Step 4: Testing and Deployment

1. **Testing and Deployment:**
   - Test the functionality of the voting system using Remix's testing tools or by writing automated tests using frameworks like Truffle.
   - Deploy the contract to a real Ethereum network (testnet or mainnet) using MetaMask or another Web3 provider.

### Summary

This decentralized voting system project demonstrates more advanced Solidity concepts, including structs, mappings, modifiers (`onlyOwner`), events, and interactions between multiple contracts (`Voting` and `Ownable`). It provides a practical example of how Solidity can be used to implement real-world applications on the Ethereum blockchain, such as voting systems with transparent and auditable results.
