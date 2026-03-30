# MARKET MAKER PLAYBOOK

## Kenya 30 Index (KEN30)

Repository: https://github.com/abba-platforms/KEN30  
Network: BNB Smart Chain (Chain ID: 56)  
Token Standard: ERC-20 Benchmark Token  

---

# 1. PURPOSE

This document defines the operational framework for market making, liquidity provisioning, and price stabilization for KEN30 across centralized exchanges.

The objective is to:

- ensure continuous two-sided liquidity
- maintain orderly markets
- anchor price behavior relative to NAV
- support efficient price discovery
- minimize volatility caused by thin liquidity

This document is intended for:

- professional market makers
- proprietary trading firms
- exchange liquidity teams
- institutional trading desks

---

# 2. CORE PRINCIPLE

KEN30 is a benchmark-driven asset.

Market making MUST be anchored to:

NAV (Net Asset Value)

NOT arbitrary price levels.

All quoting, inventory decisions, and risk controls MUST reference NAV as the fundamental value.

---

# 3. REFERENCE VARIABLES

Market makers MUST continuously track:

NAV_t  
Latest NAV value from on-chain oracle

P_bid  
Best bid price on exchange

P_ask  
Best ask price on exchange

P_mid  
(P_bid + P_ask) / 2

Spread  
P_ask - P_bid

Inventory  
Net KEN30 position held by market maker

---

# 4. QUOTING STRATEGY

## 4.1 NAV-Anchored Quoting

Quotes MUST be derived relative to NAV.

Base model:

Bid = NAV × (1 - s)  
Ask = NAV × (1 + s)

Where:

s = spread factor

---

## 4.2 Spread Regimes

### Tight Market (High Liquidity)

- Spread: 0.20% – 0.50%
- Condition: stable NAV updates, strong participation

---

### Normal Market

- Spread: 0.50% – 1.50%
- Condition: typical trading environment

---

### Volatile Market

- Spread: 1.50% – 3.00%
- Condition: NAV movement, macro volatility, low liquidity

---

Market makers MUST dynamically adjust spreads based on:

- NAV update frequency
- order book depth
- volatility conditions
- inventory exposure

---

## 4.3 Depth Provision

Market makers MUST provide:

- visible depth across multiple price levels
- layered bids and asks (minimum 3–5 levels)

Example:

Level 1: tight spread  
Level 2–5: wider spreads  

---

# 5. INVENTORY MANAGEMENT

## 5.1 Target Neutral Position

Market makers SHOULD maintain:

Inventory ≈ 0 (neutral bias)

---

## 5.2 Inventory Bands

Define acceptable exposure:

- Lower Bound: -X KEN30
- Upper Bound: +X KEN30

Where X is capital-dependent.

---

## 5.3 Inventory Adjustment Logic

If inventory > upper bound:

- reduce bids
- widen bid spread
- increase ask aggressiveness

If inventory < lower bound:

- reduce asks
- widen ask spread
- increase bid aggressiveness

---

## 5.4 NAV Drift Rebalancing

If price deviates from NAV:

- sell when price >> NAV
- buy when price << NAV

---

# 6. ARBITRAGE STRATEGY

## 6.1 NAV Arbitrage

Primary strategy:

If P_market > NAV:

- sell KEN30
- accumulate quote asset (USDT or KES)

If P_market < NAV:

- buy KEN30
- accumulate KEN30 inventory

---

## 6.2 Cross-Exchange Arbitrage (Future)

If multiple exchanges list KEN30:

- exploit price differences between venues
- maintain price convergence across markets

---

## 6.3 Oracle Lag Arbitrage

If NAV update lags market:

- temporarily widen spreads
- reduce exposure
- avoid aggressive positioning

---

# 7. LIQUIDITY PROVISION FRAMEWORK

## 7.1 Initial Capital Allocation

Market makers MUST allocate:

- base asset (KEN30)
- quote asset (USDT and/or KES)

Minimum viable liquidity per pair:

- $250,000 – $1,000,000 equivalent (recommended)

---

## 7.2 Capital Distribution

Typical structure:

- 50% quote asset
- 50% KEN30

Adjusted dynamically based on market conditions.

---

## 7.3 Order Placement Strategy

Orders MUST be:

- continuously refreshed
- adjusted based on NAV changes
- removed during abnormal conditions if necessary

---

# 8. VOLATILITY CONTROL

## 8.1 NAV Shock Handling

If NAV moves sharply:

- widen spreads immediately
- reduce order size
- re-anchor quotes to new NAV

---

## 8.2 Thin Market Conditions

If liquidity drops:

- widen spreads
- reduce exposure
- prioritize capital preservation

---

## 8.3 Order Book Imbalance

If one-sided pressure appears:

- rebalance quotes
- avoid chasing price
- allow controlled price discovery

---

# 9. RISK MANAGEMENT

## 9.1 Market Risk

Exposure to:

- Kenyan equity market movements
- macroeconomic changes

---

## 9.2 Inventory Risk

Large directional exposure due to:

- order flow imbalance
- insufficient rebalancing

---

## 9.3 Oracle Risk

NAV inaccuracies or delays may cause:

- mispriced quotes
- adverse fills

---

## 9.4 Liquidity Risk

Low participation may cause:

- wide spreads
- slippage
- inefficient pricing

---

# 10. EXECUTION CONTROLS

Market makers MUST implement:

- max position limits
- max order size limits
- spread widening triggers
- automated quote refresh intervals
- kill-switch for abnormal conditions

---

# 11. PERFORMANCE METRICS

Market makers are evaluated on:

- spread tightness
- uptime (quote presence)
- order book depth
- slippage levels
- deviation from NAV
- trade volume contribution

---

# 12. COORDINATION WITH EXCHANGE

Market makers SHOULD coordinate with exchanges on:

- liquidity incentives
- fee structures
- rebate programs
- listing timelines

---

# 13. MULTI-PAIR STRATEGY

Primary pairs:

- KEN30 / USDT
- KEN30 / KES

Market makers MUST:

- maintain consistency across pairs
- prevent cross-pair dislocations

---

# 14. OPERATIONAL REQUIREMENTS

Market makers MUST maintain:

- 24/7 trading infrastructure
- real-time NAV ingestion
- automated quoting systems
- monitoring dashboards

---

# 15. FAILURE MODES

## 15.1 Oracle Failure

- reduce quoting aggressiveness
- widen spreads
- maintain minimal liquidity

---

## 15.2 Exchange Outage

- pause trading activity
- rebalance positions after recovery

---

## 15.3 Extreme Volatility

- activate risk controls
- reduce exposure
- prioritize capital preservation

---

# 16. COMPLIANCE POSITION

KEN30 is a benchmark token.

Market makers:

- are not trading underlying equities
- are not managing a fund
- are providing liquidity in a digital benchmark instrument

---

# 17. FINAL STATEMENT

Market making for KEN30 is fundamentally NAV-driven.

Success depends on:

- disciplined quoting relative to NAV
- active inventory management
- continuous liquidity provision
- strict risk control

Market makers serve as the primary mechanism through which KEN30 achieves:

- price stability
- market efficiency
- institutional-grade tradability

---

# 18. EXECUTION INFRASTRUCTURE

## 18.1 Quoting Engine Requirements

Market makers MUST operate an automated quoting engine with the following characteristics:

- continuous bid/ask placement
- real-time NAV ingestion
- dynamic spread adjustment
- inventory-aware quoting logic

The quoting engine MUST support:

- rapid order placement
- rapid order cancellation and replacement
- deterministic behavior under varying market conditions

---

## 18.2 Quote Refresh Frequency

Recommended baseline:

- quote refresh interval: 100ms – 1000ms
- order replacement cycle MUST remain below 1 second

Under stable conditions:

- refresh every 250ms – 500ms

Under volatile conditions:

- refresh frequency MAY increase
- spreads MUST widen accordingly

---

## 18.3 NAV Ingestion Model

Market makers MUST ingest NAV using:

- event-driven subscription (NAVUpdated)
- periodic polling as fallback (every 5–15 seconds)

NAV ingestion MUST ensure:

- no missed updates
- strict roundId monotonicity validation
- timestamp verification

---

## 18.4 Order Management Logic

Market makers MUST implement:

- cancel-and-replace strategy (no stale orders)
- full order book refresh on NAV change
- partial refresh on inventory shifts

Orders MUST be:

- re-priced immediately after NAV update
- re-balanced based on inventory exposure

---

## 18.5 Latency Requirements

Target latency:

- order placement latency: < 100ms
- market data latency: near real-time
- NAV ingestion latency: < 1 second

Systems MUST be resilient to:

- network delays
- exchange API throttling
- partial outages

---

## 18.6 Failover Systems

Market makers MUST implement:

- redundant API connections
- fallback quoting logic
- automatic system recovery

If primary system fails:

- secondary system MUST assume quoting
- no prolonged market absence permitted

---

# 19. SLIPPAGE AND DEPTH MODEL

## 19.1 Depth Requirements

Market makers MUST provide depth across price levels.

Minimum structure:

- Level 1: tight spread, smaller size
- Level 2–5: increasing size, wider spreads

---

## 19.2 Slippage Targets

Recommended benchmarks:

- $10,000 trade → < 0.50% slippage
- $50,000 trade → < 1.00% slippage
- $100,000 trade → < 2.00% slippage

These targets MAY vary depending on liquidity conditions.

---

## 19.3 Order Book Density

Market makers MUST ensure:

- sufficient order density within ±2% of NAV
- layered liquidity to absorb market orders

---

## 19.4 Impact Control

If large orders enter the market:

- avoid immediate spread collapse
- maintain structured liquidity
- allow controlled price movement

Market makers MUST NOT:

- withdraw all liquidity abruptly
- create artificial price gaps

---

## 19.5 Dynamic Depth Adjustment

Depth MUST adjust based on:

- volatility
- NAV update frequency
- trading volume
- inventory exposure

---

# 20. PNL AND INCENTIVE MODEL

## 20.1 Revenue Sources

Market makers generate revenue through:

- bid-ask spread capture
- NAV arbitrage opportunities
- exchange rebates (if applicable)
- liquidity incentives (if provided)

---

## 20.2 Spread Capture

Primary revenue driver:

Profit ≈ Spread × Executed Volume

Tighter spreads increase volume but reduce margin per trade.

---

## 20.3 NAV Arbitrage Yield

Additional profit arises when:

- market price deviates from NAV

Strategies:

- sell above NAV
- buy below NAV

---

## 20.4 Inventory-Based Profit

Market makers MAY benefit from:

- directional positioning aligned with NAV trend
- controlled exposure to index movement

---

## 20.5 Expected Return Profile

Typical expectations:

- low-margin, high-frequency trading model
- consistent revenue from spread capture
- supplemental gains from arbitrage

---

## 20.6 Capital Efficiency

Return on capital depends on:

- spread discipline
- inventory control
- execution efficiency
- exchange fee structure

---

## 20.7 Incentive Alignment

Where applicable, exchanges MAY provide:

- maker rebates
- reduced trading fees
- liquidity mining programs

Market makers SHOULD optimize strategies accordingly.

---

# 21. CIRCUIT BREAKERS AND KILL-SWITCH

## 21.1 NAV Deviation Trigger

If:

| P_market - NAV | / NAV > threshold

Recommended threshold:

- 5% – 10%

Actions:

- widen spreads significantly
- reduce order size
- reassess quoting strategy

---

## 21.2 Oracle Staleness Trigger

If:

current_time - lastUpdateTimestamp > threshold

Recommended threshold:

- 120 seconds – 300 seconds

Actions:

- widen spreads
- reduce exposure
- optionally suspend aggressive quoting

---

## 21.3 Inventory Risk Trigger

If inventory exceeds defined bounds:

- halt aggressive quoting on one side
- prioritize rebalancing trades
- reduce exposure

---

## 21.4 Volatility Trigger

If rapid price movement occurs:

- widen spreads immediately
- reduce depth
- avoid overexposure

---

## 21.5 Exchange Risk Trigger

If exchange instability is detected:

- API failure
- order rejection anomalies
- latency spikes

Actions:

- reduce activity
- cancel open orders
- enter safe mode

---

## 21.6 Full Kill-Switch

Market makers MUST implement a full stop mechanism.

Trigger conditions may include:

- severe oracle failure
- extreme market dislocation
- system malfunction
- regulatory instruction

Kill-switch actions:

- cancel all active orders
- halt new order placement
- preserve capital

---

## 21.7 Recovery Protocol

After trigger conditions resolve:

- gradually reintroduce liquidity
- start with wide spreads
- normalize over time

---

End of Document
