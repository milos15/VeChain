
# Executing Your First Transaction on VeChain

VeChain is a leading blockchain platform with real-world applications across various industries, including Walmart China, BMW, DNV, and even the government of San Marino.

The core mission of the platform is to promote environmental sustainability and digital transformation through innovative blockchain solutions.

In this tutorial, we are going to help developers execute their **first transaction on VeChain blockchain**.

## Advantages of VeChain? 

By using the PoA consensus model, VeChain significantly reduces energy consumption, making the network **eco-friendly** while maintaining **strong security** and **reliability**.

Key benefits of using VeChain include:
- Interoperability
- High reliability
- Fee delegation
- Sustainable consensus
- Cost predictability
- Scalability

## What is VeChainThor? 

VeChainThor is the name of VeChain's smart contract platform. Like other blockchain platforms, this one is also built on the principle of true decentralization.

What sets VeChainThor apart is its use of the Proof of Authority (PoA) consensus mechanism. Unlike traditional systems that rely on miners or token holders, VeChainThor uses pre-selected validators, chosen based on identity and reputation, to validate transactions.


## 1. Establishing the connection to VeChainThor

To interact with VeChainThor, import ThorClient into your code editor. Once imported, instantiate an object that provides access to various methods for blockchain interaction. 

```
import { ThorClient } from "@vechain/sdk-network";
const thor = ThorClient.at("https://mainnet.vechain.org");
```

## 2. Creating a Transaction
To make changes on the blockchain create a transaction object containing the following fields: 

- to: *Recipient address*
- value: *The amount of VET or tokens*
- data: *Smart contract input data (optional)*
- gas: *Execution cost* 

Here is an example of a **basic transaction**:

```
const clause = {
  to: '0xabc123...', 
  value: '0x0de0b6b3a7640000',
  data: '0x',
};
```
## 3. Calculating the Gas for the Transaction

**Gas** is the cost of a transaction. If you want to execute a transaction on VeChain, you need to estimate how much it will cost.

Pass the clause as an argument to the ```estimateGas()``` function and store the result.

```
const gasResult = await thor.gas.estimateGas([clause]);
```

## 4. Building the Transaction Object

Wrap the transaction instructions and gas costs into an object using the ```buildTransactionBody()``` function. 

```
const txBody = await thor.transactions.buildTransactionBody([clause], gasResult.totalGas);
```

## 5. Signing the Transaction
The next step is to sign the transaction. This is done using the private key associated with the wallet that will execute the code. Signing is a four-step process:

- **Get Signer**: The wallet is initialized with the private key.
- **Sign Transaction**: The transaction is signed.
- **Build Signed Transaction Object**: The signed transaction object is built.
- **Send & Track Transaction**: The signed transaction is sent to the network and tracked.

Here is how to do it:

### Step 1: Get Signer

Import the ```SimpleWallet``` class from the VeChain SDK and instantiate a new wallet object using a private key. This wallet can be used to sign transactions and interact with the VeChain blockchain. 
```
import { SimpleWallet } from "@vechain/sdk-wallet";
const wallet = new SimpleWallet("your_private_key");
```
### Step 2: Sign the Transaction
```
const signedTx = await wallet.sign(txBody);
```
### Step 3: Send the Transaction
Once signed, the transaction is ready to be submitted to the blockchain:

```
const txId = await thor.transactions.submit(signedTx.raw);
```

### Step 4: Verifying the Transaction
After the transaction is sent, use ```getReceipt()``` function to track the transaction status. The stored value in the  receipt variable can help you determine whether the transaction was successful or not. 
```
const receipt = await thor.transactions.getReceipt(txId);
if (receipt.reverted === false) {
  console.log("Transaction was successful!");
} else {
  console.log("Transaction failed.");
}
```

## Full Code Example
Here is the full code:

```
import { ThorClient } from "@vechain/sdk-network";
import { SimpleWallet } from "@vechain/sdk-wallet";

// 1. Connect to the VeChainThor blockchain
const thor = ThorClient.at("https://mainnet.vechain.org");

// 2. Create the transaction clause
const clause = {
  to: '0xabc123...',  // Recipient address
  value: '0x0de0b6b3a7640000',  // 1 VET in wei
  data: '0x',  // Empty, no smart contract called
};

// 3. Estimate gas for the transaction
const gasResult = await thor.gas.estimateGas([clause]);

// 4. Build the transaction object
const txBody = await thor.transactions.buildTransactionBody([clause], gasResult.totalGas);

// 5. Create the wallet using your private key
const wallet = new SimpleWallet("your_private_key");

// 6. Sign the transaction
const signedTx = await wallet.sign(txBody);

// 7. Submit the signed transaction to the blockchain
const txId = await thor.transactions.submit(signedTx.raw);

// 8. Check the transaction status
const receipt = await thor.transactions.getReceipt(txId);
if (receipt.reverted === false) {
  console.log("Transaction was successful!");
} else {
  console.log("Transaction failed.");
}

```
*Written by [Milos Babic]*  
[GitHub](https://github.com/milos15) • [LinkedIn](https://www.linkedin.com/in/milosbabicns/)
