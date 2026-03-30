# Kenya 30 Index (KEN30)

## Methodology Specification

Version: 1.0  
Repository: https://github.com/abba-platforms/KEN30  

---

# 1. Overview

The Kenya 30 Index (KEN30) is a rules-based, blockchain-native benchmark designed to reference the performance of thirty leading companies listed on the Nairobi Securities Exchange (NSE).

The benchmark is constructed using a deterministic weighting methodology, with constituent composition and weights maintained on-chain through a registry contract.

KEN30 is classified as a price return equity index.

---

# 2. Index Objective

The objective of KEN30 is to provide a transparent and programmable benchmark representing the performance of leading publicly listed companies in Kenya.

The index is designed for integration into financial infrastructure, analytics systems, and digital trading environments.

KEN30 does not represent ownership of underlying securities and does not function as an investment product.

---

# 3. Index Universe

The index universe consists of companies listed on the Nairobi Securities Exchange.

Eligible companies must meet defined criteria related to:

• Market capitalization  
• Trading liquidity  
• Listing status  
• Operational continuity  

Market capitalization assessments may incorporate free-float adjustments where applicable to reflect the proportion of shares available for public trading.

---

# 4. Constituent Selection

The Kenya 30 Index includes thirty companies selected based on a combination of market capitalization and liquidity.

Selection criteria include:

• Active listing on the Nairobi Securities Exchange  
• Sufficient free-float market capitalization  
• Adequate average daily trading volume  
• Consistent trading activity  

Liquidity may be assessed using metrics such as average daily trading volume, turnover ratio, and frequency of trading activity.

Companies may be excluded if they are suspended, delisted, or fail to meet liquidity thresholds.

In cases where multiple constituents exhibit similar eligibility characteristics, selection priority may be determined based on higher liquidity metrics or consistent trading activity.

---

# 5. Constituent Count

The index maintains a fixed number of thirty constituents.

The number of constituents does not change unless modified through governance procedures.

---

# 6. Weighting Methodology

KEN30 uses a deterministic fixed-weight structure.

The index is divided into two tiers:

• Ten constituents weighted at 334 basis points each  
• Twenty constituents weighted at 333 basis points each  

Total weight:

(10 × 334) + (20 × 333) = 10000 basis points

The registry enforces that total weight equals 10000 basis points at all times.

---

# 7. Weight Assignment

Weights are assigned based on relative market importance, considering:

• Market capitalization  
• Liquidity profile  
• Sector representation  

Final weights are encoded in the on-chain registry and remain fixed between rebalancing events.

---

# 8. Index Calculation Formula

Let:

P_i = price of constituent i  
W_i = weight of constituent i  
N = number of constituents (30)  

Index Value (KEN30_t):

KEN30_t = ( Σ (P_i × W_i) ) / TOTAL_WEIGHT  

Where:

TOTAL_WEIGHT = 10000  

---

# 9. Base Value

The index is initialized with a base value of:

1.000000000000000000  

All subsequent index values represent proportional changes relative to this base level.

---

# 10. Price Inputs

Prices for each constituent are sourced from:

• Nairobi Securities Exchange official data  
• Licensed market data providers  
• Verified secondary data sources where applicable  

Prices are denominated in Kenyan Shillings (KES).

Benchmark calculations reference official market data aligned with Nairobi Securities Exchange trading hours.

---

# 11. Currency Representation

KEN30 reflects price movements in local currency terms.

The index does not incorporate foreign exchange adjustments.

---

# 12. Rebalancing

The index may undergo periodic rebalancing to maintain relevance and accuracy.

Rebalancing actions may include:

• Adjustment of constituent weights  
• Replacement of constituents  
• Updates to eligibility criteria  

Buffer rules may be applied during rebalancing to minimize unnecessary turnover. Existing constituents may be retained unless they fall materially below eligibility thresholds.

Rebalancing is executed through governance procedures and subject to timelock delay.

Rebalancing changes become effective upon execution of governance-approved registry updates following timelock delay.

---

# 13. Review Frequency

The index composition and methodology may be reviewed:

• Quarterly  
• Semi-annually  
• On an event-driven basis  

Reviews assess market developments and constituent eligibility.

---

# 14. Corporate Actions Treatment

The index may adjust for corporate actions affecting constituent securities.

Relevant corporate actions include:

• Stock splits  
• Reverse splits  
• Mergers and acquisitions  
• Delistings  
• Ticker changes  
• Rights issues  

Adjustments are implemented through registry updates via governance.

---

# 15. Treatment of Suspended Securities

If a constituent is temporarily suspended from trading:

• The last available traded price may be used  
• If suspension persists, the constituent may be removed through governance  

---

# 16. Missing Data Handling

If market data for a constituent is unavailable:

• The most recent valid price may be used temporarily  
• Persistent data gaps may trigger governance intervention  

---

# 17. Data Validation

All benchmark updates must satisfy validation rules:

• Oracle quorum requirements  
• Minimum update interval  
• Maximum allowed deviation thresholds  
• Data freshness constraints  

Invalid submissions are rejected by the protocol.

---

# 18. Index Maintenance

Index maintenance includes:

• Monitoring constituent eligibility  
• Ensuring data integrity  
• Updating methodology when required  

All changes are executed through governance procedures.

---

# 19. Methodology Transparency

The full index methodology is stored on-chain within the constituent registry.

This enables:

• Public verification of index composition  
• Independent validation of weights  
• Transparent tracking of methodology updates  

All methodology updates are version-controlled and recorded on-chain through registry versioning.

---

# 20. Governance Control

All methodology changes are governed through a timelock-controlled system.

Governance actions include:

• Registry updates  
• Rebalancing execution  
• Parameter adjustments  

No changes can be executed instantly.

The benchmark operates under a rules-based methodology and does not permit discretionary manual overrides outside of governance-controlled procedures.

---

# 21. Limitations

The index is subject to certain limitations:

• Dependence on data availability  
• Market liquidity constraints  
• Oracle submission timing  

These limitations are inherent to both traditional and digital benchmark systems.

In the event of extreme price dislocation or data anomalies, governance may temporarily suspend updates or apply corrective measures to preserve benchmark continuity.

---

# 22. Benchmark Classification

KEN30 is classified as:

• A price return equity index  
• A benchmark reference instrument  
• A programmable digital benchmark  

KEN30 is not classified as:

• A financial security  
• An exchange-traded fund  
• A derivative instrument  

---

# 23. Methodology Updates

Methodology updates may be introduced to reflect market evolution.

All updates:

• Must be approved through governance  
• Must pass timelock delay  
• Must be recorded on-chain  

---

# 24. Disclaimer

The Kenya 30 Index (KEN30) is a benchmark reference instrument designed for informational and technological purposes.

No representation or warranty is made regarding the accuracy, completeness, or timeliness of underlying market data.

The index does not represent ownership of securities, financial instruments, or investment products.

Users and integrating institutions are responsible for compliance with applicable regulatory frameworks.

---

End of Document
