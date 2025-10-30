## 🗳️ Decentralized Voting Smart Contract (`DecentralizedVoting.sol`)

<img width="1366" height="720" alt="Screenshot 2025-10-30 135937" src="https://github.com/user-attachments/assets/aca0c80b-ba12-4231-843a-c5f6a858b871" />


This Solidity smart contract implements a basic decentralized voting system with a clear voter registration process. It enforces that only explicitly registered addresses can participate in the voting process, ensuring control over who can cast a vote.

***

## 🚀 Key Features

* **Controlled Voter Registration**: Only the contract deployer (the `chairperson`) can register new addresses as eligible voters.
* **One Person, One Vote**: Registered voters can cast a vote only once.
* **Pre-Defined Proposals**: The voting options are set permanently upon contract deployment.
* **Winning Proposal Calculation**: A public function allows anyone to check the current leading proposal.

***

## ⚙️ Contract Architecture

### State Variables

| Variable | Type | Visibility | Description |
| :--- | :--- | :--- | :--- |
| `proposals` | `Proposal[]` | `public` | Array of structs holding the name and vote count for each option. |
| `isRegisteredVoter` | `mapping(address => bool)` | `public` | Tracks eligibility. `true` if the address can vote. |
| `hasVoted` | `mapping(address => bool)` | `public` | Tracks participation. `true` if the address has cast a vote. |
| `chairperson` | `address` | `public` | The address that deployed the contract and controls registration. |

### Modifiers

Modifiers are reusable checks that restrict function execution:

| Modifier | Description | Usage |
| :--- | :--- | :--- |
| `onlyChairperson` | Requires that the function caller is the contract deployer. | Used for `registerVoter`. |
| `onlyVoter` | Requires that the function caller is a registered voter. | Used for `vote`. |

### Functions

| Function | Access Control | Description |
| :--- | :--- | :--- |
| `constructor(string[] memory proposalNames)` | Special | Sets the `chairperson` and initializes the `proposals` array with the provided names. |
| `registerVoter(address voterAddress)` | `public onlyChairperson` | Adds the specified address to the `isRegisteredVoter` mapping. |
| `vote(uint256 proposalIndex)` | `public onlyVoter` | Allows an eligible voter to cast their single vote for a proposal, checking for valid index and prior voting. |
| `winningProposal()` | `public view` | Calculates and returns the name and total votes of the proposal currently in the lead. |

***

## 🛠️ Deployment and Interaction

### 1. Deployment

1.  Compile the `DecentralizedVoting.sol` file using a Solidity compiler (version $\ge 0.8.0$).
2.  Deploy the contract, providing an array of strings representing the voting options to the constructor (e.g., `["Apples", "Bananas", "Oranges"]`).
3.  The address used for deployment becomes the permanent `chairperson`.

### 2. Registration (By Chairperson)

The `chairperson` must register all eligible participants before voting can begin.

* **Function Call**: `registerVoter(address voterAddress)`
* **Example**: `registerVoter(0x123...abc)`

### 3. Voting (By Registered Voter)

Registered voters can cast their single vote. Proposal indices are zero-based (e.g., the first proposal is index `0`).

* **Function Call**: `vote(uint256 proposalIndex)`
* **Example**: To vote for the second proposal ("Bananas"), call `vote(1)`.

### 4. Viewing Results

Anyone can check the current winner and the total votes for that option.

* **Function Call**: `winningProposal()`
* **Returns**: `(string memory name, uint256 voteCount)`

***

## ⚠️ Limitations & Security Note

This is an **educational and basic implementation** intended to demonstrate core Solidity concepts (mappings, structs, modifiers, access control).

* **No Voting Period**: The contract has no mechanism to start, pause, or end voting. Voting is open indefinitely after deployment.
* **No Vote Withdrawal/Change**: Once a vote is cast, it is permanent.
* **Centralized Registration**: The registration process is fully controlled by the single `chairperson` address, which is a potential point of centralization.

**DO NOT use this contract for real-world, high-stakes voting without a thorough, professional audit and adding features such as time locks, multi-sig administration, and gas optimization.**
