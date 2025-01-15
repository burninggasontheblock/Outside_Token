# Outside Token (LOTB)

The **Outside Token (LOTB)** is an upgradable ERC-20 token contract with advanced features such as pausing, minting, and permissioned upgrades. It is built using OpenZeppelin's upgradeable contract libraries to ensure security and extendibility.

---

## Features

1. **ERC-20 Token:**
   - Name: `Outside`
   - Symbol: `LOTB`

2. **Upgradable Architecture:**
   - Built using OpenZeppelin's UUPS (Universal Upgradeable Proxy Standard).

3. **Minting:**
   - Tokens can be minted by the owner.

4. **Pausing Mechanism:**
   - The owner can pause or unpause token transfers.

5. **Permit Functionality:**
   - Supports gasless approvals via EIP-2612.

6. **Ownership Management:**
   - Only the owner has permission to perform administrative tasks like minting, pausing, and upgrades.

---

## Smart Contract Overview

### **`Outside.sol`**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.22;

import {ERC20Upgradeable} from "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";
import {ERC20PausableUpgradeable} from "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PausableUpgradeable.sol";
import {ERC20PermitUpgradeable} from "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PermitUpgradeable.sol";
import {Initializable} from "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
import {OwnableUpgradeable} from "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";
import {UUPSUpgradeable} from "@openzeppelin/contracts-upgradeable/proxy/utils/UUPSUpgradeable.sol";

contract Outside is Initializable, ERC20Upgradeable, ERC20PausableUpgradeable, OwnableUpgradeable, ERC20PermitUpgradeable, UUPSUpgradeable {
    /// @custom:oz-upgrades-unsafe-allow constructor
    constructor() {
        _disableInitializers();
    }

    function initialize(address initialOwner) initializer public {
        __ERC20_init("Outside", "LOTB");
        __ERC20Pausable_init();
        __Ownable_init(initialOwner);
        __ERC20Permit_init("Outside");
        __UUPSUpgradeable_init();
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    function _authorizeUpgrade(address newImplementation)
        internal
        onlyOwner
        override
    {}

    // The following functions are overrides required by Solidity.

    function _update(address from, address to, uint256 value)
        internal
        override(ERC20Upgradeable, ERC20PausableUpgradeable)
    {
        super._update(from, to, value);
    }
}
```

---

## Functions

### **Initialization**

#### `initialize(address initialOwner)`
Initializes the contract with:
- Token name: `Outside`
- Token symbol: `LOTB`
- Owner: `initialOwner`

### **Minting**

#### `mint(address to, uint256 amount)`
- **Description:** Mints new tokens to a specified address.
- **Access:** Only callable by the owner.

### **Pausing**

#### `pause()`
- **Description:** Pauses all token transfers.
- **Access:** Only callable by the owner.

#### `unpause()`
- **Description:** Unpauses token transfers.
- **Access:** Only callable by the owner.

### **Upgradeability**

#### `_authorizeUpgrade(address newImplementation)`
- **Description:** Allows the owner to authorize and implement contract upgrades.

---

## Deployment

1. **Prerequisites:**
   - Ethereum-compatible wallet (e.g., MetaMask).
   - Tools like Remix, Hardhat, or Truffle.

2. **Deploying:**
   - Compile the `Outside` contract.
   - Deploy the proxy and logic contracts using tools like Hardhat's `@openzeppelin/hardhat-upgrades`.

3. **Initialize:**
   - Call the `initialize` function with the owner's address.

---

## Usage

### Minting Tokens
The owner can mint tokens by calling:
```solidity
mint(address to, uint256 amount)
```

### Pausing and Unpausing
The owner can:
- Pause transfers using `pause()`.
- Resume transfers using `unpause()`.

### Upgrading the Contract
The owner can upgrade the contract logic by deploying a new implementation and authorizing the upgrade using the `_authorizeUpgrade` function.

---

## License

This project is licensed under the **MIT License**.

---

Feel free to contribute, suggest improvements, or fork this project! 🚀
