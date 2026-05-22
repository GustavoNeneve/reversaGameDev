# Game Design Document Reverse Engineering Skills

## Overview

The **reversa-gdd** suite analyzes existing game code and generates a structured **Game Design Document (GDD)** from code evidence.

## What it solves

Many game projects keep design decisions implicit in source code:

- Undocumented mechanics
- Hidden AI/state transitions
- Hard-to-audit scoring and economy rules
- Incomplete technical documentation

Reversa reconstructs these elements into a traceable documentation flow.

## Agents in the suite

### 1. `reversa-gdd` (coordinator)
- Orchestrates the full GDD pipeline
- Handles checkpoints and resume
- Supports complete, fast, and custom execution modes

### 2. `reversa-gdd-scout`
- Repository reconnaissance for game stacks
- Detects engine/framework/languages
- Locates entry points

### 3. `reversa-gdd-analyst`
- Extracts game loop and input mapping
- Analyzes physics, collisions, and AI logic

### 4. `reversa-gdd-entities`
- Documents player, enemies, NPCs, and items
- Captures attributes, states, and interactions

### 5. `reversa-gdd-flows`
- Maps screen/state transitions
- Extracts victory/defeat conditions
- Documents progression and economy

### 6. `reversa-gdd-composer`
- Composes the final Markdown GDD
- Consolidates all intermediate artifacts
- Preserves confidence markers

## Recommended flow

```text
/reversa-gdd
  └─ scout → analyst → entities → flows → composer
```

You can also run each agent directly:

```text
/reversa-gdd-scout
/reversa-gdd-analyst
/reversa-gdd-entities
/reversa-gdd-flows
/reversa-gdd-composer
```

## Output structure

```text
_reversa_sdd/
├── gdd/gdd-surface.json
├── gdd/gdd-mechanics.md
├── gdd/gdd-entities.md
├── gdd/gdd-flows.md
└── game-design-document.md
```

## Confidence scale

| Marker | Meaning |
|------|---------|
| 🟢 CONFIRMED | Directly extracted from code with citation |
| 🟡 INFERRED | Pattern-based deduction |
| 🔴 GAP | Not determinable from available code |

## Quick start

```bash
npx reversa install --agent gdd
```

Then run:

```text
/reversa-gdd
```

See also: [GDD Quick Start](../gdd-quick-start.md)
