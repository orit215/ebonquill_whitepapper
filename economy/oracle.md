---
icon: crystal-ball
---

# Oracle

## Oracle System - Technical Implementation & Economic Mechanisms

### Overview

The Oracle serves as the backbone of the in-game economy, ensuring long-term sustainability and equilibrium between Ebon Coin (the in-game currency) and <mark style="color:purple;">**$EPIC**</mark> (the native BSC token). This document provides concrete implementation details and formulas that govern the economic balancing system.

***

### Core Functions of the Oracle

#### 1. Dynamic Exchange Rate Regulation

The Oracle controls the conversion ratio between **Ebon Coin** and <mark style="color:purple;">**$EPIC**</mark> using a formula-based approach with defined parameters:

**Base Conversion Formula**

```
Rate = BaseRate × Activity_Multiplier × Supply_Adjustment × Market_Factor
```

Where:

* **BaseRate**: 1000:1 (1000 Ebon Coin = 1  $EPIC under neutral conditions)
* **Activity\_Multiplier**: 0.7 to 1.0 (adjusts based on player engagement)
* **Supply\_Adjustment**: 0.9 to 1.1 (responds to Ebon Coin generation rates)
* **Market\_Factor**: 0.95 to 1.05 (tracks $EPIC liquidity pool stability)

**Practical Rate Examples**

Based on the existing tiered system documented, rates fluctuate between **700:1** and **1000:1**:

<table><thead><tr><th width="180.33331298828125">Player Activity Level</th><th width="137.36669921875">Rate Range</th><th width="382.70001220703125">Conditions</th></tr></thead><tbody><tr><td><strong>Low Engagement</strong></td><td>1000:1</td><td>&#x3C; 500k Ebon Coin generated daily across ecosystem</td></tr><tr><td><strong>Active Player</strong></td><td>900:1</td><td>500k - 1M Ebon Coin daily generation</td></tr><tr><td><strong>High Engagement</strong></td><td>800:1</td><td>1M - 2M Ebon Coin daily generation</td></tr><tr><td><strong>Elite Contributors</strong></td><td>700:1</td><td>> 2M Ebon Coin daily generation</td></tr></tbody></table>

**Activity-Based Adjustment Calculation**

{% code expandable="true" %}
```
Activity_Multiplier = 1 - (Total_EbonCoin_Generated_Daily / Activity_Threshold) × AI_Factor
```
{% endcode %}

Where:

* **Total\_EbonCoin\_Generated\_Daily**: Sum of all Ebon Coin earned across the ecosystem in the past 24 hours
* **Activity\_Threshold**: 10,000,000 Ebon Coin (representing ecosystem capacity)
* **AI\_Factor**: 0.15 to 0.30 (ML-trained parameter adjusted weekly based on historical patterns)

**Example Calculation:**

{% code fullWidth="false" %}
```
If 1,500,000 Ebon Coin generated daily:
Activity_Multiplier = 1 - (1,500,000 / 10,000,000) × 0.25
                    = 1 - (0.15 × 0.25)
                    = 1 - 0.0375
                    = 0.9625

Final Rate = 1000 × 0.9625 = 962.5:1
            ≈ 963 Ebon Coin per 1 $EPIC
```
{% endcode %}

**Rate Bands & Clamping**

- **Normal Band:** 700:1 to 1000:1 (baseline operation).
- **Emergency Band:** 1000:1 to 1200:1 when any of the following hold:
  - `Projected_Daily_Conversions > 13,700 $EPIC`
  - `Liquidity_Health < 3.0`
  - `Light_Impact_Factor > 0.80`

```
Emergency_Multiplier ∈ [1.00, 1.20]
FinalRate = clamp(Rate × Emergency_Multiplier, 700:1, 1200:1)
```

***

#### 2. Anti-Inflation Mechanisms

The Oracle implements several concrete safeguards tied to existing game mechanics:

**A. Per-Hero Weekly Conversion Limits**

Building on the existing daily earning caps, weekly conversion limits prevent market flooding:

<table><thead><tr><th width="115.066650390625">Hero Tier</th><th width="181.89996337890625">Daily Ebon Coin Cap</th><th width="274.9000244140625">Weekly Conversion Limit (to $EPIC)</th><th>Max Weekly $EPIC Out</th></tr></thead><tbody><tr><td><strong>Common</strong></td><td>100</td><td>500 Ebon Coin</td><td>0.42 - 0.71 $EPIC</td></tr><tr><td><strong>Rare</strong></td><td>200</td><td>1,000 Ebon Coin</td><td>0.83 - 1.43 $EPIC</td></tr><tr><td><strong>Epic</strong></td><td>300</td><td>1,800 Ebon Coin</td><td>1.50 - 2.57 $EPIC</td></tr><tr><td><strong>Legendary</strong></td><td>500</td><td>3,000 Ebon Coin</td><td>2.50 - 4.29 $EPIC</td></tr></tbody></table>

{% hint style="info" %}
_Max $EPIC output varies based on the dynamic rate (700:1 to 1200:1)_
{% endhint %}

**B. Light System Integration**

The Oracle monitors Light consumption to regulate excessive farming:

```
Light_Impact_Factor = Current_Light_Consumption / Maximum_Light_Capacity
```

When Light consumption exceeds 80% of ecosystem capacity:

* Conversion rate shifts toward the 1000:1 (less favorable) end
* Additional cooldown of 6 hours applied to conversions
* **Landlord tax percentage increases by 5-10% (Epic/Legendary only; Rare remains fixed at 5%)**, but **never above the tier maximum** (Epic cap: 20%, Legendary cap: 35%), to slow Ebon Coin circulation

**Concrete Example:**

```
Legendary Dungeon Light Capacity: 1000 units
Current Light Usage across all Legendary Dungeons: 850 units

Light_Impact_Factor = 850 / 1000 = 0.85 (85% usage)

Trigger: Apply rate penalty
New conversion rate: Base × 1.15 = 1000 × 1.15 = 1150:1
```

**C. Landlord Tax Redistribution**

Leveraging the existing landlord taxation system (5% - 35% based on tier):

* **Excess** Ebon **Coin** (beyond hero daily caps) flows to Landlords
* Landlords face a **7-day conversion cooldown** on taxed Ebon Coin
* This creates a natural buffer against immediate token dumping

**Economic Flow:**

1. Player earns 600 Ebon Coin with Legendary Hero (cap: 500)
2. Player receives: 500 Ebon Coin (convertible after the per-hero cooldown: 24h for Common/Rare, 36h for Epic/Legendary)
3. Landlord receives: 100 Ebon Coin (convertible after 7-day cooldown)

**D. Landlord Weekly Conversion Caps (by Dungeon Tier)**

To prevent concentrated emissions from high-traffic dungeons, Ebon Coin received by Landlords (baseline tax + overflow) is subject to weekly conversion caps per dungeon, resetting every Monday 00:00 UTC:

| Dungeon Tier | Weekly Landlord Conversion Cap |
| ------------ | ------------------------------ |
| Rare         | 5,000 Ebon Coin                |
| Epic         | 10,000 Ebon Coin               |
| Legendary    | 20,000 Ebon Coin               |

Adaptive rule: If `Liquidity_Health < 3.0`, caps are reduced by **25%**; if `Liquidity_Health ≥ 5.0`, caps may be increased by **10%**.

***

**E. Per-Hero Conversion Cooldowns**

To prevent rapid cycling and arbitrage, each hero has a rolling cooldown after converting Ebon Coin to $EPIC:

| Hero Tier | Cooldown after Conversion |
| --------- | ------------------------- |
| Common    | 24 hours                  |
| Rare      | 24 hours                  |
| Epic      | 36 hours                  |
| Legendary | 36 hours                  |

Cooldowns apply per hero and stack with Landlord 7-day cooldowns for taxed Ebon. Attempts during cooldown are queued.

**F. Light Profitability Guard**

The Oracle enforces a floor on Light restoration ROI to avoid infinite-farming loops:

```
Expected_EPIC_Out_Per_100_Light ≤ 0.95 × EPIC_Cost_Per_100_Light
```

If violated, either `Light_Cost_Multiplier` increases by +0.05 (up to 1.20) or `FinalRate` shifts +50 toward 1000:1 temporarily.

***

#### 3. AI-Powered Fraud Detection

The Oracle employs pattern recognition to identify economic exploits:

**Multi-Account Farming Detection**

```
Fraud_Score = (Similar_Play_Patterns × 0.4) + (IP_Clustering × 0.3) + (Timing_Correlation × 0.3)
```

**Actions Triggered:**

* **Fraud\_Score > 0.7**: Flag for manual review, apply 48h conversion hold
* **Fraud\_Score > 0.85**: Temporary account suspension, Ebon Coin conversion disabled

**Market Manipulation Detection**

Monitors abnormal patterns:

* **Sudden Spikes**: If daily Ebon Coin generation increases by >200% in 24 hours → Rate shifts to 1000:1 (least favorable)
* **Coordinated Dumps**: If >5% of circulating $EPIC is sold within 1 hour → 12-hour conversion pause across ecosystem

***

#### 4. Real-Time Oracle Dashboard (for Transparency)

To build trust, the Oracle publishes real-time metrics accessible to all players:

**Public Metrics:**

* Current conversion rate (updated every 15 minutes)
* Total Ebon Coin generated (last 24 hours)
* Total Light consumption across all dungeons
* Number of active players
* $EPIC circulating supply vs. locked in staking

**Example Dashboard Display:**

<table><thead><tr><th width="179.33331298828125">ORACLE STATUS</th><th>LIVE</th></tr></thead><tbody><tr><td>Conversion Rate</td><td><strong>875:1</strong></td></tr><tr><td>Trend</td><td><strong>↓ Improving (was 920:1)</strong></td></tr><tr><td>Ebon Coin Generated (24h)</td><td><strong>1.2M</strong></td></tr><tr><td>Active Players</td><td><strong>32,450</strong></td></tr><tr><td>Next Rate Update</td><td><strong>12 minutes</strong></td></tr></tbody></table>

***

### Integration with Existing Game Systems

#### **Light Regeneration + Oracle Coordination**

The Oracle adjusts Light regeneration rates based on economic health:

```
Light_Regen_Rate = Base_Regen × (1 + Economic_Health_Bonus)

Economic_Health_Bonus = 0 to 0.20
  ├─ +0.20 when conversion rate is at 700:1 (high activity, sustainable)
  ├─ +0.10 when rate is at 850:1 (moderate activity)
  └─ 0.00 when rate is at 1000:1 (low activity, needs stimulation)
```

{% hint style="info" %}
**Effect:** During healthy economic periods, dungeons regenerate Light faster, encouraging more gameplay and sustainable farming.
{% endhint %}

#### **Light Restoration Pricing (by Dungeon Tier)**

Restoring Light requires spending <mark style="color:purple;">$EPIC</mark>. Base costs scale by tier and are adjusted by the Oracle to align ROI with economic health.

| Dungeon Tier | Base Cost per 100 Light | Notes                         |
| ------------ | ------------------------ | ----------------------------- |
| Rare         | 0.75 $EPIC               | Entry-level operations        |
| Epic         | 1.50 $EPIC               | Increased throughput          |
| Legendary    | 3.00 $EPIC               | Tournament-grade operations   |

Oracle modifier:

```
Light_Cost_Multiplier =
  1.20 when FinalRate ≤ 800:1   (high activity)
  1.00 when FinalRate ≈ 850–900:1
  0.80 when FinalRate ≥ 1000:1  (low activity)

EPIC_Cost = Base_Tier_Cost × (Light_To_Restore / 100) × Light_Cost_Multiplier
```

Bounds: `Light_Cost_Multiplier ∈ [0.80, 1.20]`.

***

#### **Seasonal Balance Adjustments**

With each seasonal expansion (introducing 4 new heroes), the Oracle recalibrates:

**Pre-Season Analysis:**

* Historical Ebon Coin generation per hero tier
* Average Light consumption patterns
* Conversion rate trends from previous season

**Post-Season Adjustments:**

* If new heroes generate 15% more Ebon Coin than predicted → Increase Activity\_Threshold by 10%
* If new content reduces player activity → Improve conversion rates temporarily (e.g., 750:1 floor for first 2 weeks)

***

### Investor Protection Mechanisms

#### **Controlled Token Emission Schedule**

Based on the documented token allocation (50% for Play & Earn):

```
Total P&E Allocation: 25,000,000 $EPIC (50% of 50M supply)

Annual Emission Target (Year 1): 5,000,000 $EPIC (20% of P&E allocation)
Daily Target: ~13,700 $EPIC

Maximum Daily Conversions = 13,700 $EPIC
```

**Oracle Enforcement:**

```
If projected daily conversions exceed 13,700 $EPIC:
├─ Increase conversion rate to 1000:1 (or higher, up to 1200:1 in extreme cases)
├─ Extend cooldown periods from 24h to 48h
└─ Reduce bonus percentages temporarily
```

#### **Liquidity Pool Monitoring**

The Oracle tracks DEX liquidity to prevent price crashes:

```
Liquidity_Health = Total_Liquidity_USD / (Daily_Conversion_Volume_USD × 30)

If Liquidity_Health < 3.0 (less than 30 days of liquidity at current pace):
├─ Reduce daily conversion limits by 25%
├─ Increase staking rewards by 10% to incentivize holding
└─ Notify community via governance proposal
```

***

### Technical Implementation Notes

#### **Data Sources for Oracle**

1. **On-Chain Data** (from BSC):
   * $EPIC price from PancakeSwap liquidity pools
   * Token holder distributions
   * Transaction volumes
2. **Off-Chain Game Data** (from game servers):
   * Ebon Coin generation rates per hero tier
   * Light consumption metrics per dungeon
   * Player activity logs (logins, session durations, dungeon completions)
3. **Machine Learning Models**:
   * **Regression Model**: Predicts next-week Ebon Coin generation based on 8-week historical data
   * **Classification Model**: Identifies fraud patterns (accuracy target: 92%+)
   * **Time-Series Forecasting**: Projects token demand for next 30 days

#### **Oracle Update Frequency**

| Metric                       | Update Frequency            | Data Source             |
| ---------------------------- | --------------------------- | ----------------------- |
| **Conversion Rate**          | Every 15 minutes            | Game servers + BSC      |
| **Fraud Detection**          | Real-time (per transaction) | Player behavior logs    |
| **Light Regeneration Rates** | Every 6 hours               | Game servers            |
| **Weekly Limits Reset**      | Every Monday 00:00 UTC      | Automated               |
| **Seasonal Adjustments**     | Start of each season        | Manual + ML predictions |

***

### Example Scenario: Oracle in Action

**Scenario:** A major tournament event drives player activity to record highs.

**Timeline:**

**Day 1 - Event Starts:**

1. Ebon Coin generation jumps to 3.5M (from normal 1M)
2. Oracle detects 250% increase
3. Conversion rate shifts: 900:1 → 1000:1
4. Light consumption hits 88% capacity
5. 6-hour conversion cooldown applied

**Day 3 - Peak Activity:**

1. Daily Ebon Coin: 4.2M
2. Oracle projects: 15,400 $EPIC daily conversions (exceeds 13,700 target)
3. Emergency adjustment: Rate → 1150:1
4. Weekly conversion limits reduced by 15% temporarily
5. Dashboard shows: "High Activity - Temporary Rate Adjustment"

**Day 7 - Event Ends:**

1. Ebon Coin generation drops to 1.8M
2. Oracle gradually improves rate: 1150:1 → 1000:1 → 875:1 (over 3 days)
3. Light consumption normalizes to 65%
4. Conversion limits return to normal

**Outcome:**

* Token emission stayed within 14,200 $EPIC/day (target: 13,700)
* $EPIC price maintained stability (±8% fluctuation)
* No signs of economic exploitation detected
