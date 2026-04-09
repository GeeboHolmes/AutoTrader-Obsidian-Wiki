---
type: concept
concept_type: smc
aliases: [Premium, Discount, Equilibrium, EQ, 50% level, PD Array]
tags: [smc, entry-quality, zones]
---

## Definition
The Premium/Discount model divides a price swing into three zones relative to its 50% midpoint (equilibrium):
- **Discount zone (below 50%):** Price is "cheap" relative to the swing — look for **longs** here.
- **Premium zone (above 50%):** Price is "expensive" relative to the swing — look for **shorts** here.
- **Equilibrium (50%):** The midpoint — neutral; often acts as S/R; use as a filter, not an entry.

The rule: buy in discount, sell in premium. This principle applies at every timeframe and to every retracement.

## How It Works
After a displacement move, price retraces. The quality of a retracement entry depends on how deep into the discount (or premium) price reaches before finding support (or resistance). An OB at 30% of the range is a higher-quality entry than one at 48%.

A "PD Array" (Price Delivery Array) is any level within the range that could cause a reaction — OBs, FVGs, liquidity levels, Fibonacci levels. They are evaluated by where they sit within the premium/discount framework.

## Identification Rules
1. Define the swing: a significant bullish or bearish leg (impulse or measured swing).
2. `equilibrium = (swing_high + swing_low) / 2`
3. **Discount:** `price < equilibrium` (lower half of the range)
4. **Premium:** `price > equilibrium` (upper half of the range)
5. For long setups: entry zones must be in the **discount** of the relevant swing.
6. For short setups: entry zones must be in the **premium** of the relevant swing.

## Fibonacci Alignment
The SMC discount/premium model aligns with Fibonacci retracement:
- 0.0 = swing low
- 0.5 = equilibrium
- 1.0 = swing high
- **Key discount levels:** 0.618, 0.705, 0.786 (deep retracements — highest quality entries)
- **Key premium levels:** 0.382, 0.236 (for shorts after a bearish displacement)

## Trading Application
- **Filter:** Only take long entries where the OB/FVG is located in the discount zone.
- **Entry quality rating:**
  - OB at 70–79% retracement (0.7–0.79 Fibonacci): excellent — deep discount, high institutional interest.
  - OB at 50–62%: acceptable.
  - OB above 50% (for a long): avoid — entering at equilibrium or premium reduces RR.
- **Combine with HTF bias:** HTF in premium + LTF short setup = high-probability short. HTF in discount + LTF long = high-probability long.

## Python Implementation Notes
- `equilibrium = (swing_high + swing_low) / 2`
- `is_discount = current_price < equilibrium`
- `is_premium = current_price > equilibrium`
- Fibonacci levels: use `swing_low + fib_level * (swing_high - swing_low)` for each Fibonacci value.
- Add `premium_discount_zone` as an attribute to each OB and FVG when detected.
- Key parameter: what defines the "swing"? Use the most recent displacement as the reference swing.

## OTE (Optimal Trade Entry) — LK Fibonacci Settings
Source: [[sources/lk-ms-golden-keys-2024]]

Within the discount zone (for longs) or premium zone (for shorts), the 0.618 and 0.786 Fibonacci levels form a high-probability sub-zone called the **OTE (Optimal Trade Entry)**. Price consistently reacts from these levels, likely due to the concentration of institutional orders there.

**Fibonacci tool settings (LK):** Configure to show only four levels:
- `0` — swing low (base)
- `0.5` — equilibrium (discount/premium split)
- `0.618` — OTE level 1 (golden ratio)
- `0.786` — OTE level 2 (deepest discount)

**Entry quality hierarchy:**
1. Price retraces to 0.786 (deepest discount) → highest quality long entry
2. Price retraces to 0.618 → excellent entry
3. Price retraces to 0.5 (equilibrium) → acceptable entry
4. Price does not reach 0.5 → do not force entry in this leg

**OTE in practice:** When a 15m CHoCH occurs in a bullish H4 leg and price pulls into the 61.8–78.6% zone, this is the OTE window. If an OB or FVG also sits in this zone, probability of reaction increases significantly.

**Python:**
```python
def get_ote_zone(swing_low, swing_high):
    """Returns the OTE zone (61.8–78.6% of the swing)."""
    return {
        'ote_low': swing_low + 0.618 * (swing_high - swing_low),
        'ote_high': swing_low + 0.786 * (swing_high - swing_low),
        'equilibrium': swing_low + 0.5 * (swing_high - swing_low)
    }

def is_in_ote(price, swing_low, swing_high, direction='long'):
    zone = get_ote_zone(swing_low, swing_high)
    if direction == 'long':
        return zone['ote_low'] <= price <= zone['ote_high']
    else:  # short: OTE is premium (above 50%, ideal at 38.2–20.6% of range FROM HIGH)
        ote_low = swing_high - 0.786 * (swing_high - swing_low)
        ote_high = swing_high - 0.618 * (swing_high - swing_low)
        return ote_low <= price <= ote_high
```

## Connections
- Related concepts: [[concepts/order-blocks]], [[concepts/fair-value-gap]], [[concepts/market-structure]]
- Part of: [[concepts/smart-money-concepts]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
| 2026-04-06 | [[sources/lk-ms-golden-keys-2024]] | Added OTE concept (0.618-0.786 zone), Fibonacci tool settings, Python implementation |
