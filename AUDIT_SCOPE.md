# AUDIT SCOPE

## Kenya 30 Index (KEN30)

Repository: https://github.com/abba-platforms/KEN30  
Version: 1.0  
Network: BNB Smart Chain  

---

# 1. Objective

This document defines the formal audit scope for the Kenya 30 Index (KEN30) smart contract system.

The purpose of the audit is to:

- validate correctness of contract logic and state transitions
- identify security vulnerabilities and attack vectors
- assess upgradeability safety under the UUPS proxy model
- evaluate oracle validation mechanisms and integrity guarantees
- verify enforcement of governance and access control restrictions

The audit is intended to support production deployment, exchange integration, and institutional participation.

---

# 2. System Description

KEN30 is a blockchain-native benchmark protocol representing a weighted index of thirty publicly listed companies on the Nairobi Securities Exchange.

The system consists of:

- ERC-20 compatible benchmark token contract
- UUPS upgradeable proxy architecture
- constituent registry contract for index methodology
- oracle-based NAV update mechanism
- timelock-controlled governance system

The protocol functions as a benchmark reference layer and does not provide custody, brokerage, or asset management functionality.

---

# 3. In-Scope Contracts

The following contracts are included within audit scope:

## 3.1 KEN30 Proxy Contract

Responsibilities:

- serves as canonical entry point
- maintains persistent storage
- delegates execution to implementation contract

Audit Focus:

- correct delegation behavior
- storage slot integrity
- upgrade routing

---

## 3.2 KEN30 Implementation Contract

Responsibilities:

- NAV state management
- oracle validation and submission handling
- role-based access control enforcement
- pause/unpause functionality
- upgrade authorization

Audit Focus:

- correctness of business logic
- validation of oracle submissions
- enforcement of access restrictions
- resilience against manipulation

---

## 3.3 Constituent Registry Contract

Responsibilities:

- storage of index constituents
- weight allocation and validation
- version-controlled methodology updates

Audit Focus:

- enforcement of total weight = 10,000 basis points
- prevention of invalid registry states
- safe interaction with main contract

---

## 3.4 Timelock Governance Contract

Responsibilities:

- scheduling and execution of administrative actions
- enforcement of governance delay
- role administration

Audit Focus:

- correct enforcement of delay mechanism
- prevention of privilege escalation
- restricted execution pathways

---

# 4. Architecture and Upgrade Assumptions

The audit assumes:

- UUPS proxy pattern is correctly implemented
- storage layout consistency is preserved across upgrades
- upgrade authority is exclusively controlled via timelock governance
- initializer functions are protected against re-execution
- no direct upgrade paths bypass governance controls

---

# 5. Trust Model

## 5.1 Oracle Trust Model

The protocol relies on a multi-signer oracle system.

Assumptions:

- oracle operators act independently
- oracle keys are securely managed
- quorum threshold prevents unilateral updates
- no majority collusion occurs among oracle operators

Risks considered:

- coordinated oracle manipulation
- delayed or missing updates
- incorrect off-chain data aggregation

---

## 5.2 Governance Trust Model

Assumptions:

- governance actions are executed exclusively through timelock
- governance participants operate in good faith
- governance proposals are publicly observable prior to execution

Risks considered:

- malicious governance proposals
- compromised governance keys
- delayed reaction to governance exploits

---

## 5.3 Registry Trust Model

Assumptions:

- registry updates are governed via timelock
- methodology inputs are validated prior to submission

Risks considered:

- incorrect weight assignments
- improper constituent inclusion/exclusion

---

# 6. Critical Security Invariants

The following invariants must hold under all conditions:

- total token supply remains fixed after initialization
- NAV updates must follow strictly increasing roundId
- oracle submissions must meet quorum requirements
- registry total weights must equal 10,000 basis points
- unauthorized accounts must not alter contract state
- upgrades must only be executed via timelock governance
- paused state must correctly restrict defined operations

---

# 7. Primary Audit Focus Areas

## 7.1 Oracle Validation

- EIP-712 signature verification correctness
- signer authorization and validation
- quorum enforcement
- replay protection via roundId
- deviation threshold enforcement
- resistance to malformed submissions

---

## 7.2 Upgradeability (UUPS)

- storage layout preservation across upgrades
- protection against storage slot collisions
- correct implementation of upgrade authorization
- initializer protection against re-execution

---

## 7.3 Access Control

- correct enforcement of role-based permissions
- prevention of unauthorized privilege escalation
- validation of role assignment mechanisms
- restriction of sensitive functions

---

## 7.4 Governance and Timelock

- correct enforcement of execution delay
- prevention of direct execution bypass
- integrity of queued transactions
- role management via governance only

---

## 7.5 NAV Integrity

- correctness of NAV state updates
- prevention of invalid or manipulated updates
- enforcement of update constraints
- correct handling of stale or delayed submissions

---

## 7.6 Registry Integration

- validation of constituent weights
- safe interaction between contracts
- prevention of invalid registry states

---

## 7.7 Pause Mechanism

- correct activation and deactivation behavior
- restriction of defined operations during pause
- absence of unintended side effects

---

# 8. Out-of-Scope Components

The following are explicitly excluded:

- off-chain data providers and data accuracy
- oracle infrastructure outside contract logic
- centralized exchange systems and order matching engines
- frontend applications and user interfaces
- regulatory or legal classification
- economic performance of the benchmark

---

# 9. Dependencies and Constraints

The protocol depends on:

- reliable off-chain market data aggregation
- secure oracle key management
- proper governance key security
- timely oracle updates

Constraints:

- NAV accuracy depends on external data
- governance introduces execution delays
- benchmark does not represent financial ownership

---

# 10. Testing Requirements

The audit should be supported by comprehensive testing, including:

- unit tests for all core functions
- oracle submission simulations
- invalid signature and quorum failure scenarios
- upgrade simulations ensuring storage integrity
- access control validation tests
- pause/unpause behavior validation
- edge case and boundary condition testing

---

# 11. Expected Audit Deliverables

The audit is expected to produce:

- detailed vulnerability report categorized by severity
- technical analysis of identified issues
- recommended remediation actions
- verification of resolved findings (if applicable)
- overall system security assessment

---

# 12. Audit Readiness Statement

The KEN30 protocol is structured to support institutional-grade security auditing.

The system provides:

- clearly defined architecture
- modular contract design
- transparent governance model
- publicly verifiable smart contracts
- comprehensive documentation

The protocol is considered ready for formal third-party audit engagement.

---

# 13. Codebase Scope and Version Lock

This audit scope applies to a specific snapshot of the KEN30 codebase.

- Repository: https://github.com/abba-platforms/KEN30  
- Branch: main (or explicitly specified branch)  
- Commit Hash: TO BE SPECIFIED AT AUDIT INITIATION  

Only the code within the defined commit hash is considered in scope.

Any modifications to the codebase after the specified commit hash invalidate this audit scope and require re-evaluation.

---

# 14. Threat Model Classification

The audit evaluates the system against the following threat categories:

- external attacker (unauthorized address attempting to manipulate contract state)
- malicious oracle participant attempting to submit invalid or manipulated NAV data
- compromised governance actor attempting unauthorized upgrades or parameter changes
- upgrade-based attack vectors including storage collision and implementation substitution
- data integrity manipulation through malformed or replayed oracle submissions
- denial-of-service conditions including oracle inactivity or improper use of pause functionality

Each threat class must be evaluated against the protocol’s defensive mechanisms and control structures.

---

# 15. Custody and Asset Exposure Statement

The KEN30 protocol does not:

- custody user funds
- hold underlying financial assets
- manage pooled capital
- provide asset management or investment services

The smart contract operates exclusively as a benchmark reference system.

As a result, the audit scope does not include fund custody risk, asset safeguarding mechanisms, or financial asset protection considerations.

---

End of Document
