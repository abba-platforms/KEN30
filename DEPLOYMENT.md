# Kenya 30 Index (KEN30)

## Deployment Specification

Version: 1.0  
Repository: https://github.com/abba-platforms/KEN30  
Network: BNB Smart Chain Mainnet  
Chain ID: 56  
Proxy Standard: ERC1967 (UUPS-compatible)

---

# 1. Overview

This document defines the deployed contract architecture, on-chain addresses, verification status, and integration references for the Kenya 30 Index (KEN30).

KEN30 is deployed as a proxy-based benchmark protocol on BNB Smart Chain Mainnet. The deployment is structured to support transparent verification, controlled upgradeability, deterministic benchmark methodology, and institutional integration.

This specification serves as the canonical deployment reference for exchanges, auditors, market infrastructure operators, and technical integrators.

---

# 2. Network Environment

KEN30 is deployed on:

Network: BNB Smart Chain Mainnet  
Chain ID: 56  

The protocol uses BNB Smart Chain as the execution environment for benchmark logic, governance controls, and public contract verification.

---

# 3. Contract Architecture

KEN30 is deployed using a proxy-based upgrade architecture.

The deployed system consists of the following components:

• Proxy contract  
• Implementation contract  
• Constituent registry contract  
• Timelock governance contract  

The proxy contract is the canonical user-facing contract address. All integrations, exchange listings, wallet views, and public interactions should reference the proxy contract.

The implementation contract contains the executable logic of the protocol and may be upgraded only through governance-controlled procedures.

Direct interaction with the implementation contract is not supported and may result in unintended behavior.

The constituent registry contract stores the benchmark composition and weights.

The timelock contract governs administrative actions, including upgrades, registry changes, and protocol configuration changes.

---

# 4. Deployed Mainnet Contracts

## 4.1 Canonical KEN30 Proxy

Contract Type: ERC1967 Proxy  
Role: Primary user-facing benchmark contract  
Address: 0x83A73BCb66D8Bb4866d274D228F07952B34D725a  

BscScan:  
https://bscscan.com/token/0x83A73BCb66D8Bb4866d274D228F07952B34D725a

This is the canonical live KEN30 contract address.

All end-user integrations must reference this address.

---

## 4.2 KEN30 Implementation

Contract Type: Logic Contract  
Role: Contains executable benchmark logic  
Address: 0x7D543a8689542f90F8cDABB5fbFb6b8d9CDBaCD7  

BscScan:  
https://bscscan.com/address/0x7D543a8689542f90F8cDABB5fbFb6b8d9CDBaCD7#code

This address should not be used as the operational integration endpoint.

---

## 4.3 Constituent Registry

Contract Type: Registry Contract  
Role: Stores constituent names and weights  
Address: 0x4abA5c3C445b0EB5327Ce81c154101B0335B67E4  

This contract maintains the on-chain benchmark composition and weighting structure.

---

## 4.4 Timelock Controller

Contract Type: Governance Contract  
Role: Enforces delayed administrative execution  
Address: 0xc2EA242B04B5F2100B1003A589729976ACcA1Ca7  

This contract governs administrative operations including upgrades, methodology changes, and controlled protocol actions.

---

# 5. Verification Status

The KEN30 proxy and implementation contracts have been verified on BscScan.

Verified contracts:

• KEN30 Proxy  
• KEN30 Implementation  

Verification ensures:

• Public source code transparency  
• ABI consistency  
• Independent bytecode inspection  
• Reproducible contract review  

The verification status supports exchange due diligence, audit processes, and institutional technical review.

---

# 6. ABI and Interface

The contract ABI is available via BscScan and within the repository.

Integrators must use the proxy contract ABI for all interactions.

---

# 7. Key Read Functions

Typical read operations include:

• totalSupply()  
• balanceOf(address)  
• name()  
• symbol()  
• decimals()  

Benchmark-specific read functions are accessible via the verified ABI and may include registry-linked data retrieval and benchmark reference outputs.

---

# 8. Event Emissions

The contract emits standard ERC-20 events:

• Transfer  
• Approval  

Additional protocol-level events may be emitted for:

• Benchmark updates  
• Governance execution  
• Registry changes  

---

# 9. Access Control

Administrative permissions are enforced using role-based access control.

Roles may include:

• DEFAULT_ADMIN_ROLE  
• UPGRADER_ROLE  
• PAUSER_ROLE  
• ORACLE_ROLE  

All privileged roles operate under governance control via the timelock contract.

---

# 10. Proxy Model Clarification

The proxy contract is the stable integration endpoint.

The implementation contract contains the executable logic and is accessed through delegatecall.

This architecture provides:

• Contract continuity at the proxy address  
• Controlled upgradeability  
• Separation of interface and logic  
• Governance-enforced protocol evolution  

---

# 11. Upgrade Governance

The upgrade path is strictly controlled.

All upgrades must:

• Be proposed through governance  
• Be scheduled via the timelock contract  
• Observe mandatory delay periods  
• Be executed through governance-controlled transactions  

No direct or immediate upgrades are permitted.

---

# 12. Registry Deployment Notes

The constituent registry is deployed separately to preserve transparency and modularity.

The registry stores:

• Constituent identifiers  
• Constituent names  
• Weight allocations  
• Version history  

Total weight is constrained to 10,000 basis points.

All registry updates are governance-controlled.

---

# 13. Governance Deployment Notes

The timelock controller governs all sensitive actions.

Governed actions include:

• Protocol upgrades  
• Registry updates  
• Oracle configuration  
• Emergency administrative responses  

All actions are subject to delayed execution.

---

# 14. Token Parameters

Benchmark Name: Kenya 30 Index  
Benchmark Symbol: KEN30  
Decimals: 18  
Maximum Supply: 10,000,000,000 KEN30  

The total supply is fixed at initialization.

No inflationary mechanism exists post-deployment.

---

# 15. Integration Endpoint Guidance

Primary integration address:

0x83A73BCb66D8Bb4866d274D228F07952B34D725a

Applicable for:

• Wallet integrations  
• Exchange listings  
• Data feeds  
• Analytics systems  
• Custodial infrastructure  

The implementation contract must not be used for integration.

---

# 16. Dependencies

The KEN30 contracts are built using OpenZeppelin upgradeable libraries to ensure security, standardization, and compatibility with industry tooling.

---

# 17. Deployment Reproducibility

The protocol was deployed using Hardhat.

Verification can be reproduced using:

npx hardhat verify --network bsc <contract_address>

Proxy verification requires:

• Proxy address  
• Implementation address  
• Initialization parameters  

---

# 18. Operational Security Notes

The deployment incorporates:

• Timelock-enforced governance  
• Role-based access control  
• Proxy upgrade restrictions  
• Verified contract transparency  
• Deterministic supply  
• Oracle-controlled updates  

---

# 19. Operational Considerations

All interactions require BNB Smart Chain gas fees.

Gas costs depend on network conditions and transaction complexity.

---

# 20. Benchmark Canonical Reference

Canonical KEN30 contract:

0x83A73BCb66D8Bb4866d274D228F07952B34D725a

This address must be used in all official references.

---

# 21. Documentation Alignment

This document should be read alongside:

• WHITEPAPER.md  
• METHODOLOGY_SPEC.md  
• LIQUIDITY_STRATEGY.md  

Together, these define the full KEN30 protocol stack.

---

# 22. Disclaimer

This document is provided for technical and informational purposes.

Smart contract verification does not eliminate risk.

Users and institutions are responsible for independent validation, integration testing, and regulatory compliance.

---

End of Document
