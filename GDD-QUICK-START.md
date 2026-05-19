# 🎮 GDD Reverse Engineering — Quick Start Guide

## What Was Added

Six new AI skills for automatic Game Design Document (GDD) generation from game source code:

1. **reversa-gdd** — Orchestrator (Main entry point)
2. **reversa-gdd-scout** — Game code reconnaissance
3. **reversa-gdd-analyst** — Game mechanics analysis
4. **reversa-gdd-entities** — Player/Enemies/Items analysis
5. **reversa-gdd-flows** — Game flow & progression analysis
6. **reversa-gdd-composer** — Final GDD synthesis

## How to Use

### Fast Track (5 minutes)

```
Open your AI agent and type:
/reversa-gdd
```

Then choose:
- **Completo** (Full, 20+ min) — All analysis
- **Rápido** (Fast, 10 min) — Scout + Analyst only
- **Personalizado** (Custom) — Pick specific phases

### What You Get

```
_reversa_sdd/
├── gdd/gdd-surface.json           # Game engine detected
├── gdd/gdd-mechanics.md           # Game loop, inputs, physics
├── gdd/gdd-entities.md            # Player, enemies, items
├── gdd/gdd-flows.md               # Game states, economy
└── game-design-document.md        # ← YOUR FINAL GDD!
```

## Understanding the GDD

Your final `game-design-document.md` contains:

| Section | What's Inside |
|---------|---------------|
| **Executive Summary** | Quick overview of the game |
| **1. Visão Geral** | Game concept, genre, tech stack |
| **2. Fluxo** | Game states, win/loss conditions |
| **3. Mecânicas** | Controls, physics, AI |
| **4. Entidades** | Player, enemies, NPCs, items |
| **5. Progressão** | Levels, difficulty, progression |
| **6. Análise Técnica** | Architecture, data, performance |
| **7. Rastreabilidade** | Confidence levels, gaps, Q&A |

## Confidence Levels

Everything is marked with confidence:

- **🟢 CONFIRMED** — Found directly in code with citation
- **🟡 INFERRED** — Deduced from patterns, may be wrong
- **🔴 GAP** — Can't determine from code, needs human input

## Manual Mode (If You Want Control)

Run each agent individually:

```
/reversa-gdd-scout            # 2 min
/reversa-gdd-analyst          # 5 min
/reversa-gdd-entities         # 4 min
/reversa-gdd-flows            # 4 min
/reversa-gdd-composer         # 3 min
```

Between each, you'll see `CONTINUAR` — press it to proceed.

## Supported Game Engines

- ✅ Unity (C#)
- ✅ Godot (GDScript)
- ✅ Unreal (C++)
- ✅ Custom Engines (any language)
- ✅ Web Games (JavaScript/TypeScript)

## Real Example

Your 2D platformer game:

```
INPUT:
  Your Unity project with:
  - 45 C# scripts
  - 2D physics
  - 5 levels
  - 2 enemy types

ANALYSIS (20 minutes):
  Scout finds:        Unity 2021, C#, 2D platformer
  Analyst finds:      60 FPS loop, 4 key controls, 5 speed
  Entities finds:     Player 100 HP, Walker enemies
  Flows finds:        5 levels linear, coins economy
  Composer creates:   Professional GDD

OUTPUT:
  📄 game-design-document.md (ready to share with your team!)
```

## What It Extracts

✅ **Game Loop** — How the game updates each frame
✅ **Input Mapping** — Every keyboard/mouse/gamepad control
✅ **Physics Values** — Speed, gravity, acceleration
✅ **Collision System** — All colliders and their purposes
✅ **AI Behavior** — Enemy state machines and logic
✅ **Game Flow** — Menu → Playing → Game Over
✅ **Win Conditions** — What makes you win
✅ **Lose Conditions** — What makes you lose
✅ **Economy** — How coins/points/XP work
✅ **Progression** — Level structure and difficulty

## What It CAN'T Extract (Gaps)

❌ Game story/narrative (unless documented in code)
❌ Artistic direction (unless referenced)
❌ Audio design (unless explicitly coded)
❌ Marketing/positioning
❌ Unknown/undocumented systems

These become 🔴 GAPS in your GDD for human validation.

## Next Steps

1. **Run the analysis** — `/reversa-gdd`
2. **Review the GDD** — Read `game-design-document.md`
3. **Fill gaps** — Have your team answer the 🔴 questions
4. **Share it** — Export to Confluence/Notion/Google Docs
5. **Use it** — As reference for refactoring, porting, etc.

## Need Help?

- Read full documentation: `docs/agentes/gdd-reverse-engineering.md` (English)
- Read full documentation: `docs/agentes/gdd-engenharia-reversa.pt.md` (Português)
- Each skill has detailed instructions in its `SKILL.md` file
- Check `agents/reversa-gdd*/SKILL.md` for technical details

## Examples of Output

### Game Loop Found
```
🟢 CONFIRMED: `GameManager.cs:45`
- Update() called every frame (60 FPS)
- FixedUpdate() for physics (50 times/sec)
```

### Input Mapping
```
| Action | Control | Code |
|--------|---------|------|
| Move | A/D keys | Input.GetAxis("Horizontal") |
| Jump | Spacebar | Input.GetKeyDown(KeyCode.Space) |
| Attack | Mouse Click | Input.GetMouseButtonDown(0) |
```

### Enemy AI
```
Walker Enemy — 🟢 State Machine Found
- Patrol: Walks between points
- Chase: Pursues player if < 8 units
- Attack: Deals 5 damage
- Die: Drops 100 XP
```

---

**You now have professional GDD generation built-in to your Reversa installation!** 👾

Start with: `/reversa-gdd`

**Made by reversaGameDev + sandeco**
