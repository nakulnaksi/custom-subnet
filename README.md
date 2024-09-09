# 🎮 DeFi Empire: A DeFi Kingdom Clone on Avalanche

Welcome to the **DeFi Empire** project! In this project, we build a **DeFi Kingdom Clone** on the Avalanche network using a custom **EVM Subnet**. The project involves creating a decentralized finance (DeFi) game where players can **battle**, **explore**, and **trade** digital assets, earning rewards in the form of tokens.

## 🚀 Project Overview

This project focuses on setting up the foundational smart contracts for a DeFi Kingdom-style game, including:

- **Custom EVM Subnet Setup** on Avalanche
- **ERC-20 Token Contract** for in-game currency
- **Vault Contract** for staking and liquidity pools

The goal is to deploy these contracts and set the groundwork for a DeFi gaming experience where users can interact with the game, trade assets, and earn rewards.

---

## 📦 Prerequisites

Before proceeding, make sure you have the following installed:

- **Node.js** (v14 or higher)
- **npm** (Package Manager)
- **Solidity** (for smart contract development)
- **Remix IDE** (or an equivalent Solidity IDE)
- **Metamask** (for connecting to your Subnet)
- **Avalanche CLI** (for EVM subnet deployment)

## 📝 Smart Contracts

### 1. **ERC-20 Token Contract**

The ERC-20 contract serves as the foundation for the in-game currency.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    string public name = "Game Token";
    string public symbol = "GAMETOK";
    uint8 public decimals = 18;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint amount) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

### 2. **Vault Contract**

The Vault contract allows players to deposit tokens and participate in game rewards.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint _amount) external {
        uint shares = (_amount * totalSupply) / token.balanceOf(address(this));
        totalSupply += shares;
        balanceOf[msg.sender] += shares;
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        totalSupply -= _shares;
        balanceOf[msg.sender] -= _shares;
        token.transfer(msg.sender, amount);
    }
}
```

---

## 🔧 Steps to Deploy

### 1. **Create EVM Subnet on Avalanche**
   - Follow the Avalanche documentation to create a custom **EVM subnet**.
   - Set up your subnet with a custom **native token** for in-game transactions.

### 2. **Connect to Metamask**
   - Add your **EVM Subnet** to Metamask by entering the required details (RPC URL, chain ID, etc.).
   - Ensure that the custom subnet is selected in Metamask.

### 3. **Deploy Smart Contracts**
   - Open **Remix IDE**.
   - Compile the ERC-20 and Vault smart contracts.
   - Deploy the contracts to your custom subnet using the **Injected Web3** provider.

### 4. **Test Contracts**
   - Interact with the deployed contracts using the Remix interface.
   - Mint tokens, deposit into the Vault, and test other interactions.

---

## 🛠️ Tools & Technologies Used

- **Avalanche CLI**
- **Solidity** (Smart contract language)
- **Remix IDE**
- **Metamask** (For wallet interactions)

---

## 💡 Tips

- Make sure to **test your contracts** thoroughly before final submission.
- Use **Metamask** to switch between networks and verify the deployment process.
- Take your time with the **ERC-20** and **Vault contracts**, as they are essential for the game's economy.

---

## 📧 Contact & Support

If you have any questions or run into issues, feel free to reach out to the project maintainers.
