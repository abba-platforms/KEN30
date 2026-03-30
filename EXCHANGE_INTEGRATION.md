# EXCHANGE INTEGRATION SPECIFICATION

## Kenya 30 Index (KEN30)

Repository: https://github.com/abba-platforms/KEN30  
Network: BNB Smart Chain (Chain ID: 56)  
Token Standard: ERC-20 Compatible Benchmark Token  
Decimals: 18  

---

# 1. PURPOSE

This document defines the full technical integration specification required for centralized exchanges (CEX), trading platforms, and market makers to integrate the Kenya 30 Index (KEN30) token.

The objective is to provide a deterministic and unambiguous framework for:

- token listing
- NAV ingestion
- pricing model alignment
- market structure implementation
- operational monitoring
- risk handling

This document is intended for:

- exchange backend engineers
- listing committees
- market structure teams
- quantitative trading desks
- market makers

---

# 2. SYSTEM CHARACTERISTICS

KEN30 is a blockchain-native benchmark token representing a weighted index of thirty companies listed on the Nairobi Securities Exchange.

The token has the following properties:

- ERC-20 compatible
- fixed total supply
- no minting after deployment
- no burn mechanics
- no rebasing
- no transfer restrictions
- no embedded fee logic

KEN30 is not:

- an equity instrument
- a fund
- a derivative contract
- a claim on underlying securities

KEN30 is a reference benchmark token whose value is externally derived from an oracle-updated NAV.

---

# 3. CONTRACT LAYER INTEGRATION

## 3.1 Canonical Contract Address

KEN30 Proxy Contract (User-Facing Address):

<TO BE INSERTED>

All exchange integrations MUST interact exclusively with the proxy contract.

The implementation contract MUST NOT be used for integration.

---

## 3.2 Token Interface Compliance

KEN30 conforms to the ERC-20 interface.

Required functions:

- totalSupply()
- balanceOf(address)
- transfer(address,uint256)
- transferFrom(address,address,uint256)
- approve(address,uint256)
- allowance(address,address)

Return values follow standard ERC-20 expectations.

---

## 3.3 Transfer Semantics

- Transfers are deterministic
- No hooks or callbacks
- No transfer fees
- No blacklist or whitelist logic
- No pausing of transfers unless explicitly implemented and documented

Exchanges can treat KEN30 as a standard ERC-20 asset for custody and movement.

---

## 3.4 Deposit Handling

Standard ERC-20 deposit flow:

1. User sends KEN30 to exchange deposit address
2. Exchange monitors Transfer events
3. Confirmations required: recommended >= 12 blocks
4. Credit user account after confirmation threshold

No special decoding or custom logic required.

---

## 3.5 Withdrawal Handling

Standard ERC-20 withdrawal flow:

1. User initiates withdrawal
2. Exchange signs transaction
3. Exchange sends transfer() call to contract
4. Transaction is broadcast to BNB Smart Chain

Gas requirements are standard ERC-20 levels.

---

# 4. NAV (NET ASSET VALUE) INTEGRATION

## 4.1 NAV Definition

NAV represents the benchmark value of the Kenya 30 Index.

NAV is:

- updated via oracle consensus
- stored on-chain
- independent of exchange market price

---

## 4.2 NAV Storage Variables

The contract maintains:

- latest NAV value
- timestamp of update
- roundId

---

## 4.3 NAV Retrieval

Exchanges MUST retrieve NAV via:

- direct contract read calls (preferred)
- or event-based updates

Recommended implementation:

- poll NAV at defined intervals (for example, every 30-60 seconds)
- subscribe to NAVUpdated events for real-time updates

---

## 4.4 NAV Event Structure

Primary event:

event NAVUpdated(uint256 nav, uint256 timestamp, uint256 roundId);

Event parameters:

- nav: benchmark value scaled to 18 decimals
- timestamp: block timestamp of update
- roundId: strictly increasing identifier

Exchanges SHOULD subscribe to NAVUpdated events for real-time updates.

---

## 4.5 NAV Update Characteristics

- NAV updates are discrete, not continuous
- Updates depend on oracle submission
- Update frequency may vary
- roundId MUST strictly increase

---

## 4.6 NAV Validation Rules (Exchange Side)

Exchanges SHOULD enforce the following validation logic:

- reject NAV with non-increasing roundId
- ignore stale updates
- validate timestamp freshness if required by internal systems

---

# 5. PRICING MODEL IMPLEMENTATION

## 5.1 Dual-Value Model

KEN30 has two distinct values:

1. NAV (Reference Value)
2. Market Price (Exchange-Traded Value)

These MUST NOT be conflated.

---

## 5.2 Exchange Price Formation

Exchange price is determined by:

- order book supply and demand
- liquidity conditions
- market maker activity

The protocol DOES NOT enforce price parity with NAV.

---

## 5.3 NAV Display Requirements

Exchanges are strongly recommended to display:

- KEN30 NAV (reference)
- KEN30 Price (market)

Clear labeling is required.

NAV SHOULD be labeled as:

Benchmark Reference Value (NAV)

---

## 5.4 Price Deviation Behavior

Deviation between NAV and market price is expected.

Exchanges MUST NOT:

- auto-correct price to NAV
- enforce peg logic
- suspend trading solely because price differs from NAV

---

# 6. MARKET STRUCTURE

## 6.1 Primary Trading Pairs

Required primary trading pairs include:

- KEN30 / USDT
- KEN30 / KES

---

## 6.2 Optional Trading Pairs

Optional pairs may include:

- KEN30 / BTC
- KEN30 / ETH

---

## 6.3 Order Book Model

KEN30 is intended to trade using a standard centralized exchange limit order book.

No custom matching engine logic is required.

---

## 6.4 Tick Size and Precision

Recommended:

- price precision: 4-8 decimals
- quantity precision: full 18 decimals internally

Exchanges MAY adapt visible frontend precision according to venue conventions.

---

# 7. MARKET MAKING REQUIREMENTS

## 7.1 Role of Market Makers

Market makers are expected to provide:

- liquidity
- bid-ask continuity
- price stability around benchmark value

---

## 7.2 Liquidity Model

Initial liquidity may be supported through:

- treasury-led seeding
- institutional allocations
- market maker participation
- exchange-supported liquidity programs

---

## 7.3 Spread Guidelines

Recommended operational approach:

- maintain spreads as close to NAV as commercially viable
- adjust spreads dynamically during volatility
- provide sufficient visible depth

The protocol does not enforce spread limits.

---

## 7.4 Inventory Management

Market makers should manage inventory relative to:

- latest NAV
- exchange price
- expected volatility
- available liquidity

---

# 8. RISK MANAGEMENT

## 8.1 Oracle Risk

NAV depends on oracle updates.

Risks include:

- delayed updates
- incorrect submissions
- oracle collusion
- stale benchmark data

---

## 8.2 Market Risk

KEN30 reflects the performance of an equity benchmark and is subject to:

- macroeconomic changes
- constituent price volatility
- regional market shocks

---

## 8.3 Liquidity Risk

Low liquidity may produce:

- wide spreads
- price dislocation from NAV
- reduced market efficiency

---

## 8.4 Operational Risk

Operational risks include:

- missed NAV updates
- incorrect monitoring configuration
- improper decimal handling
- incomplete event indexing

---

# 9. MONITORING REQUIREMENTS

Exchanges MUST implement monitoring for:

- NAVUpdated events
- roundId progression
- abnormal NAV deviation
- oracle inactivity
- governance upgrades

Recommended monitoring outputs include:

- stale NAV alerts
- sudden update gap alerts
- roundId discontinuity alerts
- contract upgrade alerts

---

# 10. GOVERNANCE AWARENESS

The protocol uses timelock governance.

Exchanges MUST monitor:

- contract upgrade events
- governance proposals where possible
- registry updates
- oracle configuration changes

Exchanges SHOULD maintain procedures for re-validation after governance-triggered upgrades.

---

# 11. FAILURE MODES

## 11.1 Oracle Failure

If NAV stops updating:

- the last valid NAV remains active
- trading may continue
- exchanges SHOULD flag NAV as stale if thresholds are exceeded

---

## 11.2 Extreme Price Deviation

If market price diverges materially from NAV:

- no protocol-level intervention occurs
- price remains market-driven
- market makers and exchange surveillance systems handle the condition

---

## 11.3 Governance Event

If an implementation upgrade occurs:

- exchanges SHOULD re-verify contract behavior
- exchanges SHOULD re-check ABI compatibility
- exchanges SHOULD validate event continuity

---

# 12. COMPLIANCE POSITION

KEN30:

- does not represent ownership of listed companies
- does not provide dividends
- does not constitute a fund
- does not constitute a debt claim
- does not represent a claim on underlying assets

Classification:

Benchmark Reference Token

Exchanges must conduct independent legal and regulatory review within their jurisdiction.

---

# 13. INTEGRATION CHECKLIST

Before listing KEN30, exchanges MUST:

- verify proxy contract address
- confirm ERC-20 compliance
- test deposits and withdrawals
- validate NAV retrieval
- implement NAV event subscriptions
- configure monitoring systems
- define supported trading pairs
- coordinate liquidity provisioning
- validate decimal normalization
- define stale NAV handling logic

---

# 14. SUPPORT

For integration support:

Email: admin@ken30.com  
WhatsApp: +264-81-250-9027  

---

# 15. FINAL STATEMENT

KEN30 is designed for seamless integration into centralized exchange infrastructure without requiring custom execution logic.

All asset custody and transfer behavior follow standard ERC-20 conventions.

Additional integration responsibility exists at the NAV and monitoring layer, where exchanges must consume benchmark reference values and distinguish them from market-traded prices.

---

# 16. CONTRACT FUNCTION INTERFACE SPECIFICATION

## 16.1 Required Read Functions

Exchanges MUST integrate directly against the KEN30 proxy contract and rely on explicit function calls for benchmark data retrieval.

The following read-only functions MUST be supported by the contract:

function nav() external view returns (uint256)

function lastUpdateTimestamp() external view returns (uint256)

function roundId() external view returns (uint256)

---

## 16.2 Function Definitions

nav()

Returns the latest benchmark Net Asset Value (NAV) as a uint256.

- Value is scaled to 18 decimal places
- Represents the current index level
- Updated via oracle consensus mechanism

lastUpdateTimestamp()

Returns the UNIX timestamp in seconds of the last accepted oracle update.

- Used for staleness detection
- Exchanges SHOULD validate recency of updates

roundId()

Returns a monotonically increasing identifier for each accepted NAV update.

- MUST strictly increase with every valid update
- Used for ordering and validation of updates

---

## 16.3 Integration Requirements

Exchanges MUST:

- query nav() for benchmark reference value
- query roundId() to ensure monotonic progression
- query lastUpdateTimestamp() for freshness validation

Exchanges MUST NOT:

- derive NAV from external calculations
- override contract-reported NAV

---

## 16.4 ABI Dependency

Exchanges SHOULD retrieve the verified ABI from:

https://bscscan.com

using the canonical proxy contract address.

All integrations MUST use the proxy ABI, not the implementation ABI.

---

# 17. DECIMAL NORMALIZATION AND PRICE HANDLING

## 17.1 NAV Decimal Format

The nav() function returns values in fixed-point format with 18 decimals.

Example raw value:

1234567890000000000

This represents:

1.234567890

---

## 17.2 Normalization Rule

Exchanges MUST normalize NAV using:

NAV_display = NAV_raw / 1e18

Failure to normalize will result in incorrect pricing by a factor of 10^18.

---

## 17.3 Price Engine Integration

When constructing trading systems:

- NAV MUST be converted to floating-point or decimal representation
- internal calculations MAY retain high precision
- UI display SHOULD round appropriately (for example, 4-8 decimals)

---

## 17.4 Pair-Specific Handling

For KEN30 / USDT:

- normalized NAV can be directly compared to USDT price

For KEN30 / KES:

- exchanges MAY apply FX conversion if needed
- or allow market-driven pricing independent of NAV

---

## 17.5 Precision Consistency

Exchanges MUST ensure:

- no truncation of NAV before normalization
- consistent decimal handling across all systems
- alignment between backend, matching engine, and UI

---

# 18. INITIAL STATE AND BOOTSTRAP CONDITIONS

## 18.1 Pre-Initialization State

Before the first successful oracle update:

- nav() MAY return zero or default value
- roundId() MAY be zero
- lastUpdateTimestamp() MAY be unset or zero

---

## 18.2 Exchange Handling Requirements

Exchanges MUST implement safeguards:

- DO NOT display NAV if roundId() == 0
- DO NOT rely on nav() if returned value == 0
- WAIT for the first valid oracle update before enabling NAV-based features

---

## 18.3 Trading Activation

Trading MAY proceed independently of NAV availability.

However:

- NAV display SHOULD be disabled until the first valid update
- pricing SHOULD remain entirely market-driven until NAV becomes available

---

## 18.4 First Valid State Definition

A valid NAV state is defined as:

- roundId() > 0
- nav() > 0
- lastUpdateTimestamp() > 0

All three conditions MUST be satisfied before the exchange treats NAV as initialized.

---

## 18.5 Stale Data Handling

Exchanges SHOULD define a staleness threshold.

Example logic:

- if current_time - lastUpdateTimestamp() > internal threshold
- NAV is considered stale

Recommended actions:

- flag NAV as stale in the UI
- continue trading without interruption
- continue order matching using market-driven pricing

---

## 18.6 Recovery From Initialization State

Once the first valid NAV is observed:

- enable NAV display
- enable NAV-based analytics
- enable NAV freshness monitoring
- maintain normal integration workflow thereafter

No manual intervention is required after a valid state is reached.

---

End of Document
