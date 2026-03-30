# ORACLE SPECIFICATION

## Kenya 30 Index (KEN30)

Repository: https://github.com/abba-platforms/KEN30
Version: 1.0

---

# 1. Overview

The KEN30 Oracle System is responsible for the secure, accurate, and verifiable submission of benchmark index values (NAV) to the KEN30 smart contract.

The oracle framework ensures that all benchmark updates are:

- derived from reliable market data
- cryptographically verified
- validated through multi-party consensus
- resistant to manipulation or unilateral control

The oracle system operates as a critical component of the KEN30 benchmark infrastructure, bridging off-chain market data and on-chain execution.

---

# 2. Oracle Architecture

The oracle system follows a multi-operator, quorum-based architecture.

Core components include:

- data aggregation layer
- oracle signer nodes
- message signing protocol
- on-chain verification logic

Each oracle operator independently retrieves, processes, and signs benchmark data before submission.

The smart contract validates submissions based on quorum requirements and cryptographic signatures.

---

# 3. Data Source Hierarchy

The oracle system uses a structured hierarchy of data sources.

## 3.1 Primary Data Sources

Primary data is sourced directly from:

- Nairobi Securities Exchange (NSE) official price feeds
- official closing prices and authorized market data publications

These sources serve as the authoritative reference for constituent pricing.

## 3.2 Secondary Data Sources

Secondary data sources may include:

- licensed financial data providers
- regulated market data vendors
- institutional-grade data aggregators

Secondary sources are used for cross-verification and redundancy.

## 3.3 Fallback Data Logic

In cases where primary and secondary data are unavailable:

- the most recent valid traded price may be used
- fallback usage is subject to validation constraints
- fallback conditions must not exceed defined staleness thresholds

Fallback logic ensures continuity without compromising benchmark integrity.

---

# 4. NAV Calculation Responsibility

Oracle operators are responsible for computing the Net Asset Value (NAV) of the KEN30 index.

The NAV is calculated using:

KEN30_t = ( SUM (P_i x W_i) ) / TOTAL_WEIGHT

Where:

P_i = price of constituent i  
W_i = weight of constituent i  
TOTAL_WEIGHT = 10,000 basis points  

Each oracle independently performs this calculation prior to signing.

---

# 5. Oracle Operator Requirements

Oracle operators must meet defined operational and security standards.

Requirements include:

- access to reliable market data sources
- secure key management practices
- high-availability infrastructure
- ability to perform deterministic NAV calculations
- compliance with protocol-defined update schedules

Operators must maintain system integrity and ensure consistent data submission.

---

# 6. Signing and Message Format

Oracle updates are submitted as cryptographically signed messages.

## 6.1 Signing Standard

The system utilizes:

- EIP-712 typed structured data signing

This ensures:

- domain separation
- replay protection
- verifiable message integrity

## 6.2 Message Structure (Canonical Fields)

Each oracle submission must include the following fields:

- nav (uint256): computed index value  
- timestamp (uint256): time of calculation (EAT-aligned)  
- roundId (uint256): monotonically increasing sequence identifier  
- chainId (uint256): network identifier for domain separation  
- indexId (string or bytes32): identifier for KEN30  
- signer (address): oracle signer address  

These fields are encoded into the structured message and signed off-chain.

The canonical schema for these fields is formally defined in Section 16.3.

---

# 7. Quorum Requirements

The oracle system enforces multi-signature validation.

## 7.1 Minimum Quorum

A predefined minimum number of oracle signatures is required for a valid update.

Example configuration:

- 3-of-5 oracle quorum

## 7.2 Validation Logic

The smart contract verifies:

- authenticity of each signature  
- uniqueness of signers  
- quorum threshold satisfaction  

Only updates meeting quorum requirements are accepted.

---

# 8. Update Frequency and Timing

Oracle updates are submitted at defined intervals.

## 8.1 Update Frequency

Updates may be:

- intraday (for example, hourly)  
- periodic (for example, daily close)  

The exact frequency is governed by protocol configuration.

## 8.2 Time Standard

All timestamps are standardized to:

- East Africa Time (EAT)

## 8.3 Minimum Interval

The contract enforces:

- minimum time between updates  

This prevents excessive or spam submissions.

---

# 9. Data Validation Rules

Submitted updates must pass validation checks.

## 9.1 Price Deviation Limits

Updates must fall within predefined deviation thresholds relative to the previous NAV.

## 9.2 Staleness Constraints

Updates exceeding maximum allowed staleness are rejected.

## 9.3 Sequence Enforcement

Submissions must follow correct sequencing to prevent replay or out-of-order updates.

---

# 10. Failure Handling

The oracle system includes mechanisms for handling failure scenarios.

## 10.1 Insufficient Quorum

If quorum is not met:

- update is rejected  
- previous NAV remains active  

## 10.2 Data Unavailability

If data sources are unavailable:

- fallback pricing may be used within defined limits  
- extended unavailability may trigger governance action  

## 10.3 Oracle Downtime

If one or more oracle operators fail:

- system continues if quorum is still achievable  
- governance may replace inactive operators  

---

# 11. Security Considerations

The oracle system incorporates multiple security safeguards.

## 11.1 Multi-Operator Design

Prevents single-point control over benchmark updates.

## 11.2 Cryptographic Verification

Ensures authenticity and integrity of submitted data.

## 11.3 Timelock Governance

Oracle configuration changes must pass through governance delay.

## 11.4 Key Management

Oracle operators must maintain secure private key storage and rotation policies.

---

# 12. Oracle Governance

Oracle configuration is governed through the timelock-controlled governance system.

Governance actions include:

- adding or removing oracle operators  
- modifying quorum thresholds  
- updating validation parameters  

All changes require:

- proposal submission  
- timelock delay  
- execution  

---

# 13. Integration with Smart Contract

The oracle system interfaces directly with the KEN30 smart contract.

The contract performs:

- signature verification  
- quorum validation  
- NAV update execution  
- event emission  

The oracle does not directly modify state without validation.

---

# 14. Audit and Transparency

All oracle submissions are:

- recorded on-chain  
- publicly verifiable  
- auditable through transaction history  

This ensures full transparency of benchmark updates.

---

# 15. Summary

The KEN30 oracle system provides a secure, verifiable, and decentralized mechanism for updating benchmark values.

Through structured data sourcing, multi-operator consensus, cryptographic validation, and governance controls, the oracle framework ensures the integrity and reliability of the KEN30 index within institutional financial environments.

---

# 16. Operational and Validation Parameters

## 16.1 Oracle Signer Admission and Governance

Oracle operators are admitted through governance-controlled processes.

Eligibility criteria include:

- access to reliable and verifiable market data  
- institutional-grade infrastructure  
- secure key management practices  
- deterministic NAV computation capability  

---

## 16.2 Oracle Accountability and Removal Conditions

Operators may be removed for:

- invalid data submission  
- quorum participation failure  
- key compromise  
- protocol violations  

---

## 16.3 NAV Update Message Schema (Canonical Definition)

The canonical message schema is defined as:

NAVUpdate {
    uint256 nav;
    uint256 timestamp;
    uint256 roundId;
    uint256 chainId;
}

The fields defined in Section 6.2 must map directly to this schema.

---

## 16.4 Replay Protection Mechanism

Replay protection is enforced through:

- strictly increasing roundId  
- rejection of duplicate messages  
- hash tracking of processed submissions  

---

## 16.5 Deviation Threshold Parameters

Example configuration:

- maximum deviation: +/-5%

Out-of-range submissions are rejected.

---

## 16.6 Update Finality

Accepted updates are:

- final  
- immutable  
- forward-only  

---

## 16.7 Oracle Incentive Model

Participation may be:

- internal  
- institutional  
- service-based  

No on-chain incentive is enforced.

---

## 16.8 Monitoring and Alerting

Monitoring includes:

- update frequency  
- deviation tracking  
- quorum participation  

---

## 16.9 Disaster Recovery

In failure scenarios:

- updates may be paused  
- last valid NAV remains active  
- governance restores operations  

---

## 16.10 Time Synchronization

Time reference:

- block.timestamp  

Constraints:

- maximum drift  
- staleness limits  

---

End of Document
