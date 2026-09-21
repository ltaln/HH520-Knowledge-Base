# Probability Layer

## Purpose

Select the main WDL direction from validated probability information.

## Source Priority

1. `page_probability` from 10027s fusion section, when all home/draw/away values are valid.
2. De-vigged market 1X2 odds as fallback.

Partial page probability must not silently replace a complete market distribution.

## Direction

Highest normalized probability among:
- home
- draw
- away

## Existing concentration field

The production probability layer also calculates the gap between the highest and second-highest probabilities.

Important distinction:

Research Decision Filter factor `probability_concentration` is bucketed by **top probability level**:
- <40%
- 40–49%
- 50–59%
- >=60%

Do not confuse this with the production layer's top-minus-second gap field.

## Authority

Probability Layer defines direction.
Value Layer and Decision Filter may assess value/risk, but must not silently change the selected direction.
