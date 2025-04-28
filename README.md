# Traditional voting system, merkle tree based and zero-knowledge based
## Overview
This project benchmarks three different blockchain-based voting mechanisms:
- Traditional Voting (Simple smart contract voting)
- Merkle Tree-based Voting (Scalable proof of eligibility)
- Zero-Knowledge Proof-based Voting (Privacy-preserving voting)

The goal is to analyze and compare efficiency, scalability, and privacy guarantees across all three systems by measuring:
- Gas Usage
- Transaction Fee
- Block Size Impact
- Time Taken to Vote
  
:woman_technologist: ## Setup Instructions
Prequisite: Solidity, Node, Hardhat installed

### Step 1: Install Backend Dependencies
```
npm install
```

### Step 2: Add Test Networks
You're using two test networks:
- **Immutable zkEVM Testnet**
- **Ethereum Holesky Testnet**


In `hardhat.config.js`
```
require("@nomicfoundation/hardhat-toolbox");
const fs = require("fs");
const path = require("path");

task("clean", "Clean the artifacts, cache, and ignition deployment directories", async () => {
  const dirs = ["artifacts", "cache", "ignition/deployments"];
  dirs.forEach((dir) => {
    const dirPath = path.resolve(__dirname, dir);
    if (fs.existsSync(dirPath)) {
      fs.rmSync(dirPath, { recursive: true, force: true });
      console.log(`${dir} folder deleted`);
    }
  });
});

module.exports = {
  solidity: "0.8.27",
  networks: {
    zkTest: {
      url: "https://rpc.testnet.immutable.com",
      chainId: 13473,
      accounts: ["YOUR_PRIVATE_KEY"]
    },
    ethTest: {
      url: "https://ethereum-holesky-rpc.publicnode.com",
      chainId: 17000,
      accounts: ["YOUR_PRIVATE_KEY"]
    },
    hardhat: {
      chainId: 1337,
    },
  },
};
```
>Replace YOUR_PRIVATE_KEY with your actual private key.

In `.env`file
```
IMMUTABLE_ZKEVM_RPC_PROVIDER=https://rpc.testnet.immutable.com
ETH_PROVIDER=https://ethereum-holesky-rpc.publicnode.com

ETH_PRIVATE_KEY=YOUR_PRIVATE_KEY
IMMUTABLE_ZKEVM_PRIVATE_KEY=YOUR_PRIVATE_KEY
```
#### Step 3: Clean Previous Deployments 
If you've changed your contract and want a fresh deployment:
```
npx hardhat clean
```
#### Step 4: Deploy Contracts
**1. Deploy to Holesky**
```
npx hardhat ignition deploy ignition/modules/ETHdeploy.js --network ethTest
```
After deployment, you'll see two contract addresses in the terminal. Use them as follows:
Replace in `routes/votingContract.js` [line 11]
```
const contractAddress = 'PASTE_ETH_VOTING_CONTRACT_ADDRESS_HERE';
```
Replace in `routes/merkleVotingRoutes.js` [line 14]
```
const contractAddress = 'PASTE_MERKLEVOTING_CONTRACT_ADDRESS_HERE';
```
**2. Deploy to Immutable zkEVM**
```
npx hardhat ignition deploy ignition/modules/ZKEVMdeploy.js --network zkTest
```
Use the address shown in the terminal to replace the following:
Replace in `routes/zkEvmVotingContract.js` [line 11]
```
const contractAddress = 'PASTE_ZKEVM_CONTRACT_ADDRESS_HERE';
```

### Step 5: Start the Backend Server
```
node server.js
```

### Step 6: Frontend Setup
Go to your frontend project directory and run:
```
npm install
npm run dev
```
> This will start your frontend development server, typically running at [http://localhost:3000](http://localhost:3000).
