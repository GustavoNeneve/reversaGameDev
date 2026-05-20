# Eco Water Estimator

## Overview

`/reversa-eco` estimates the water footprint of an AI workload from **tokens, words, characters or reference prompts**, then compares that estimate with **YouTube streaming ranges**.

## What it solves

This agent is useful when you want a simple environmental comparison such as:

- How much water does this token volume approximately represent
- How that estimate compares with 15 minutes of YouTube
- How many tokens would be needed to reach the same water range as 1 hour of streaming

## Important distinction from pricing

`/reversa-pricing-size` does **not** count tokens or LOC.

If your goal is **token based water estimation**, use `/reversa-eco`.

## Accepted inputs

You can start from one of these bases:

- `tokens`
- `palavras`
- `caracteres`
- `prompts`

The agent normalizes everything to tokens with these reference conversions:

- `100 tokens = 75 words`
- `1 token = 4 characters`
- `1 reference prompt = 1000 tokens`

## Core formula

```text
reference_prompt_equivalents = tokens_total / 1000
water_ml = reference_prompt_equivalents * 0.26
water_l = water_ml / 1000
```

## YouTube comparison

The report compares the result with these ranges:

- `15 min of YouTube = 1 to 3 L`
- `1 hour of streaming = 2 to 12 L`

Because YouTube is expressed as a range, the comparison is also shown as a range.

## Output structure

```text
_reversa_sdd/
└── _eco/
    ├── eco.json
    └── eco.md
```

## Output highlights

The generated report includes:

- informed base and normalized tokens
- estimated words and characters
- estimated water in mL and L
- equivalent number of reference prompts
- percentage against 15 minutes of YouTube
- percentage against 1 hour of streaming
- token thresholds required to match YouTube ranges

## Example

`1,000,000 tokens` becomes:

- `1000` reference prompts
- `260 mL` of estimated water
- about `8.67% to 26%` of `15 min` of YouTube

## Disclaimer

This is an educational estimate. It is useful for relative comparison, not for formal environmental auditing.
