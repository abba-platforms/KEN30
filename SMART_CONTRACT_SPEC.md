# SMART CONTRACT SPECIFICATION

## Kenya 30 Index (KEN30)

Repository: https://github.com/abba-platforms/KEN30       
Version: 1.0       
Network: BNB Smart Chain       
Standard: ERC-20 Compatible       

---

# 1. Overview

The KEN30 smart contract system provides the canonical on-chain implementation of the Kenya 30 Index.

The contract is responsible for:

- maintaining the benchmark token supply
- receiving and validating oracle updates
- storing the latest NAV (Net Asset Value)
- enforcing governance controls
- enabling secure and transparent interaction with the benchmark

The system is designed for institutional-grade reliability, transparency, and upgradeability.

---

# 2. Contract Architecture

The KEN30 protocol uses a proxy-based upgradeable architecture.

## 2.1 Components

The system consists of:

- Proxy Contract (user-facing)
- Implementation Contract (logic layer)
- Constituent Registry Contract
- Timelock Governance Contract

## 2.2 Proxy Pattern

The proxy contract:

- serves as the canonical address for all interactions
- delegates execution to the implementation contract
- preserves storage across upgrades

The implementation contract:

- contains all executable logic
- can be upgraded through governance

## 2.3 Upgrade Standard

The system follows:

- UUPS (Universal Upgradeable Proxy Standard)

Upgrade operations:

- must be initiated through governance
- must pass timelock delay
- cannot be executed directly by external accounts

---

# 3. Token Specification

## 3.1 Token Properties

- Name: Kenya 30 Index
- Symbol: KEN30
- Decimals: 18
- Standard: ERC-20 compatible

## 3.2 Supply Model

- Maximum Supply: 10,000,000,000 tokens
- Minting: executed once at initialization
- Post-deployment minting: disabled

## 3.3 Transfer Behavior

The token supports standard ERC-20 functionality:

- transfer
- approve
- transferFrom

Optional extensions may include pause functionality.

---

# 4. NAV State Management

## 4.1 NAV Storage

The contract maintains:

- latest NAV value
- timestamp of last update
- roundId for sequencing

## 4.2 NAV Update Function

NAV updates are submitted via oracle consensus and must pass validation.

## 4.3 NAV Finality

Once accepted:

- NAV updates are immutable
- updates can only move forward
- rollback is not permitted

---

# 5. Oracle Integration

## 5.1 Oracle Interface

Oracle submissions include:

- NAV value
- timestamp
- roundId
- signatures

## 5.2 Validation Logic

The contract enforces:

- signature verification
- signer authorization
- quorum threshold
- replay protection
- deviation constraints

## 5.3 Replay Protection

Replay prevention is enforced via:

- strictly increasing roundId
- duplicate rejection

---

# 6. Role-Based Access Control

## 6.1 Core Roles

- DEFAULT_ADMIN_ROLE
- ORACLE_ROLE
- PAUSER_ROLE
- UPGRADER_ROLE

## 6.2 Role Permissions

DEFAULT_ADMIN_ROLE:

- manages roles

ORACLE_ROLE:

- submits updates

PAUSER_ROLE:

- pauses contract

UPGRADER_ROLE:

- executes upgrades

## 6.3 Governance Control

All roles are controlled via timelock governance.

---

# 7. Pausable Mechanism

## 7.1 Behavior

When paused:

- NAV updates are halted

## 7.2 Use Cases

- oracle compromise
- system vulnerability

---

# 8. Security Architecture

## 8.1 Reentrancy Protection

Reentrancy guards are applied to critical functions.

## 8.2 Input Validation

All inputs are validated for correctness.

## 8.3 Signature Verification

All signatures are validated using EIP-712.

## 8.4 Upgrade Protection

Upgrades require governance execution.

---

# 9. Constituent Registry Integration

## 9.1 Registry Usage

The contract references the registry for weights.

## 9.2 Validation

Total weight must equal 10,000 basis points.

---

# 10. Governance Integration

## 10.1 Governance Actions

- upgrade contract
- manage oracles
- adjust parameters

## 10.2 Timelock

All actions require delay.

---

# 11. Event Emissions

Key events:

- NAVUpdated
- OracleUpdateRejected
- RoleGranted
- Paused
- Upgraded

---

# 12. Gas Optimization

The contract minimizes redundant storage operations.

---

# 13. Failure Handling

## 13.1 Invalid Submission

Rejected if validation fails.

## 13.2 Oracle Downtime

Last NAV remains active.

## 13.3 Emergency

System may be paused.

---

# 14. Upgrade Lifecycle

1. proposal
2. delay
3. execution

---

# 15. Audit and Verification

The contract is publicly verifiable and auditable.

---

# 16. Summary

The KEN30 smart contract provides a secure and upgradeable benchmark implementation.

---

# 17. Contract Implementation and Execution Specification

## 17.1 Storage Layout

The contract maintains the following core storage variables:

- uint256 public nav
- uint256 public lastUpdateTimestamp
- uint256 public roundId
- mapping(address => bool) public isOracle
- uint256 public quorum
- address public registry
- bool public paused

Storage layout must remain consistent across upgrades.

---

## 17.2 Core Functions

### updateNAV

Inputs:

- nav
- timestamp
- roundId
- signatures

Validation:

- verify EIP-712 signatures
- validate oracle signers
- enforce quorum
- check roundId increment
- enforce deviation threshold

State Changes:

- update nav
- update timestamp
- update roundId

Events:

- NAVUpdated

---

### pause

- restricted to PAUSER_ROLE
- sets paused state

---

### unpause

- restricted to PAUSER_ROLE
- restores normal operation

---

### upgradeTo

- restricted to UPGRADER_ROLE
- executed via timelock

---

## 17.3 Oracle Execution Flow

1. receive oracle payload
2. decode message
3. verify signatures
4. validate signers
5. check quorum
6. validate roundId
7. check deviation
8. update NAV

---

## 17.4 Parameter Definitions

Default parameters:

- quorum: 3-of-5
- deviation threshold: +/-5%
- minimum update interval: defined by governance
- staleness limit: defined by governance

---

## 17.5 Event Definitions

event NAVUpdated(uint256 nav, uint256 timestamp, uint256 roundId);
event OracleUpdateRejected(bytes reason);
event Paused();
event Unpaused();
event Upgraded(address implementation);

---

## 17.6 Error Conditions

The contract reverts under:

- invalid signature
- insufficient quorum
- invalid roundId
- excessive deviation
- unauthorized caller

---

## 17.7 Interfaces

### Oracle Interface

Defines submission format for NAV updates.

### Registry Interface

Returns:

- constituent weights
- validation status

---

## 17.8 Pause Behavior

When paused:

- NAV updates are blocked
- transfers may continue unless restricted

---

## 17.9 Upgrade Authorization

Only timelock-governed UPGRADER_ROLE may execute upgrades.

---

End of Document
