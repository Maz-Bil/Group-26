# Group-26
 Blockchain-Enabled Pharmaceutical Supply Chain Provenance


Group Members:                 | Teachers & TAs
Gautham Jayakrishnan           | Swathi Punathumkandi
Ridham Ka Patel                | Sandipan De
Tushmi Sharma                  | Chaitanya Ashok Patel
Jonathan Steddom               | Meet Vyas
Maz Bilgrami                   |


# PharmaSupplyChain

## 📌 Project Overview
This project implements a blockchain-based supply chain management system to track pharmaceutical products and raw materials throughout their lifecycle.  

The system ensures **transparency, authenticity, and traceability** in the pharmaceutical ecosystem using Ethereum smart contracts written in Solidity.  

Key features include:
- Immutable record keeping for raw materials and products  
- Role-based access control using OpenZeppelin’s `AccessControl`  
- Event logging for complete traceability  
- Product provenance verification  
- Security protections via OpenZeppelin’s `ReentrancyGuard`  

---

## 🛠️ Tech Stack
- **Solidity (v0.8.x)** – Smart contract language  
- **Remix IDE** – Online IDE for writing, compiling, and deploying contracts  
- **MetaMask** – Wallet for deploying and interacting with contracts on Ethereum networks  
- **Ethereum** – Blockchain platform (testnets like Polygon Amoy/Sepolia recommended)  
- **OpenZeppelin Contracts** – Standard libraries for access control and security  

---


## ⚙️ Environment Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/pharma-supply-chain.git
   cd pharma-supply-chain

2. Install Dependencies
    ```bash
    forge install OpenZeppelin/openzeppelin-contracts@v4.9.3
    curl -L https://foundry.paradigm.xyz | bash
    foundryup
3. Initialize a Foundry Project.
   ```bash
   mkdir myproject
   cd myproject
   forge init .
4. Remappings May Be Required. Edit the foundry.toml to include the following:
   ```bash
   [profile.default]
   remappings = [
    "@openzeppelin/=lib/openzeppelin-contracts/",
    "forge-std/=lib/forge-std/src/"
   ]
    
5. Compile Contract and Start Blockchain Locally with Anvil. This will print private keys/addresses for testing.
   ```bash
   forge build
   anvil

6. Leave Terminal Open from previous step. Open new terminal in project root.
   ```bash
   forge create src/SupplyChain.sol:SupplyChain \
     --rpc-url http://127.0.0.1:8545 \
     --private-key <PRIVATE_KEY> //This will be generate from anvil when starting contract

 7. Assign Roles to addresses. From running anvil, you can assign roles to the generated addresses.
    ```bash
    // Get role hash
    cast keccak "REGULATOR_ROLE"
    // Admin can assign role to other addressess
    cast send <USER_ADDRESS> "grantRole(bytes32,address)" \
      0x7d4d... 0xABC123... \    // Role... Address to assign
      --private-key <PRIVATE_KEY>
    // Verify role was assigned
    cast call <USER_ADDRESS> "hasRole(bytes32,address)" 0x7d4d... 0xABC123...

 8. Ready to test. The following are call formats 
    ```bash
    // Read only functions
    cast call <USER_ADDRESS> "myGetter() returns (uint256)"
    // Write functions
    cast send <USER_ADDRESS> "myFunction(uint256)" 42 \
      --private-key <PRIVATE_KEY>

📘 Instructor Q&A

Below are clarifications to common conceptual questions asked during review.

❓ What consensus mechanism does the project use?

The system is deployed on the Ethereum network, which uses Proof of Stake (PoS). For this proof-of-concept, we do not modify consensus. Instead, we focus on demonstrating supply-chain logic, immutability, event logging, and authorization. In production, the system could run on Ethereum L2s or a permissioned chain like Hyperledger Fabric.

❓ How many channels are used?

The system uses a single shared ledger (“one channel”). Ethereum does not support Hyperledger-style channels. Instead, role-based access control ensures data separation within one canonical smart contract.

❓ What makes this system different from existing blockchain supply-chain solutions?

Our design introduces:

Layered authenticity
Raw materials → batches → transfers → final dispensing
Each stage is validated and cryptographically logged.

Individualized role permissions
Manufacturers, distributors, pharmacists, and regulators are individually assigned roles through grantRole(), enabling fine-grained access control.

Event-driven traceability
Every major action emits structured logs, enabling full auditing via transaction receipts.

End-to-end provenance
The system traces the complete lifecycle:
raw inputs → manufacturing → custody transfers → pharmacist dispensing.

This goes beyond many systems that only track products at a single stage.


❓ Why use blockchain instead of a traditional database?

Blockchain provides:

Immutability – no participant can modify historical supply-chain data.

Trustless coordination – manufacturers, distributors, and pharmacies do not need to trust each other.

Regulatory alignment – DSCSA and similar regulations require secure, auditable traceability.

Cryptographic integrity – event logs encode actor identity, timestamps, and data changes.

These guarantees cannot be replicated by a centralized database.

❓ What did you learn from this project?

We learned how blockchain can be used to create a tamper-proof, event-driven supply-chain system with fine-grained role management. Implementing the project deepened our understanding of smart contract development, event logs, transaction flows, and the importance of layered authenticity in pharmaceutical provenance. We also learned how to design systems that balance usability, security, gas efficiency, and regulatory requirements.
