# Implementation Notes: GDD Reverse Engineering Skills

## Overview

This document explains the architecture and design decisions for the new GDD (Game Design Document) reverse engineering skill suite added to reversaGameDev.

## Problem Statement

The user requested a comprehensive skill that performs reverse engineering analysis of game source code and generates a professional Game Design Document. The original request included a detailed prompt describing how to act as a Senior Game Designer and Software Engineer.

## Solution Architecture

Instead of creating a single monolithic skill, we decomposed the problem into **6 specialized, coordinated skills** following the Reversa framework pattern:

```
User invokes: /reversa-gdd
       ↓
[Orchestrator decides workflow]
       ↓
[Scout] → Surface reconnaissance
       ↓
[Analyst] → Deep mechanical analysis
       ↓
[Entities] → Character/AI analysis
       ↓
[Flows] → Progression/economy analysis
       ↓
[Composer] → Final GDD synthesis
       ↓
Output: game-design-document.md
```

## Design Principles

### 1. **Modularity**
Each skill has a single responsibility:
- **Scout**: Find framework, structure, entry points
- **Analyst**: Extract mechanics, physics, AI
- **Entities**: Document characters, enemies, items
- **Flows**: Map progression, economy, states
- **Composer**: Synthesize and format final output

### 2. **Composability**
Skills can be:
- Run together (via Orchestrator) for full analysis
- Run individually for focused analysis
- Run in sequence with manual checkpoints
- Paused and resumed between sessions

### 3. **Reversa Compliance**
All skills follow Reversa patterns:
- Read `.reversa/state.json` for context
- Use `output_folder` for output paths
- Respect `doc_level` settings
- Mark confidence levels (🟢🟡🔴)
- Preserve original project (non-destructive)

### 4. **Engine Agnostic**
Analysis works with any game technology:
- **Unity**: Detects MonoBehaviour patterns, Update/FixedUpdate
- **Godot**: Detects _ready/_process patterns, GDScript
- **Unreal**: Detects AActor/ACharacter patterns
- **Custom**: Generic analysis of loops, structs, state patterns
- **Web**: Detects framework libraries (Phaser, Three.js)

### 5. **Confidence-Based**
Every finding is marked:
- 🟢 **CONFIRMED**: Code citation with file:line
- 🟡 **INFERRED**: Pattern-based deduction
- 🔴 **GAP**: Unknown, requires human validation

## File Structure

```
agents/
├── reversa-gdd/                  # Orchestrator
│   └── SKILL.md                  # 8.9 KB
├── reversa-gdd-scout/            # Reconnaissance
│   └── SKILL.md                  # 9.0 KB
├── reversa-gdd-analyst/          # Deep analysis
│   └── SKILL.md                  # 11.4 KB
├── reversa-gdd-entities/         # Entity analysis
│   └── SKILL.md                  # 11.1 KB
├── reversa-gdd-flows/            # Flow analysis
│   └── SKILL.md                  # 11.3 KB
└── reversa-gdd-composer/         # Synthesis
    └── SKILL.md                  # 10.7 KB

docs/agentes/
├── gdd-reverse-engineering.md    # English docs (10.3 KB)
└── gdd-engenharia-reversa.pt.md  # Portuguese docs (10.4 KB)

GDD-QUICK-START.md               # Quick reference (5 KB)
```

## How Each Skill Works

### reversa-gdd (Orchestrator)

**Role**: Central coordinator
**Size**: ~8.9 KB
**Responsibilities**:
- Presents pipeline to user
- Offers execution modes (full/fast/custom)
- Manages checkpoints in state.json
- Coordinates handoff between agents
- Displays final summary

**Key Directives**:
- Non-destructive (won't overwrite existing GDD without permission)
- Checkpoint-aware (can resume from where it left off)
- User-guided (pauses for CONTINUAR between phases)

---

### reversa-gdd-scout (Reconnaissance)

**Role**: Surface mapping
**Size**: ~9.0 KB
**Analyzes**:
- Folder structure and naming conventions
- Technology stack detection
- Engine/framework identification
- Language detection
- Entry point location
- Dependency discovery

**Output**: `gdd-surface.json`

**Detection Patterns**:
- Unity: Looks for `Assets/`, `ProjectSettings/`, `*.csproj`
- Godot: Looks for `project.godot`, `*.gd`, `*.tscn`
- Unreal: Looks for `*.uproject`, `Source/`, `Content/`
- Custom: Analyzes structure and patterns

---

### reversa-gdd-analyst (Deep Analysis)

**Role**: Mechanics extraction
**Size**: ~11.4 KB
**Analyzes**:
- Game loop (Update, FixedUpdate, Render cycles)
- Input handling (keyboard, mouse, gamepad, touch)
- Physics parameters (velocity, gravity, acceleration, friction)
- Collision system (rigidbodies, colliders, triggers)
- State management (enums, booleans, state machines)
- AI behavior (enemy patterns, decision trees, FSMs)

**Output**: `gdd-mechanics.md`

**Citation Format**:
Each finding includes filename:line_number for traceability
Example: `PlayerController.cs:42`

---

### reversa-gdd-entities (Entity Analysis)

**Role**: Character and entity documentation
**Size**: ~11.1 KB
**Analyzes**:
- Player character (attributes, states, abilities)
- Enemy types (behavior, stats, loot)
- NPCs (dialogue, interactions, missions)
- Items (types, effects, economy)
- Entity hierarchies and relationships

**Output**: `gdd-entities.md`

**Structures Documented**:
- Attribute tables (HP, speed, damage, etc.)
- State machines (with transitions)
- Behavior patterns
- Interaction types
- Equipment/inventory systems

---

### reversa-gdd-flows (Flow Analysis)

**Role**: Progression and systems
**Size**: ~11.3 KB
**Analyzes**:
- Game state transitions (Menu → Playing → GameOver)
- Win conditions (what triggers victory)
- Lose conditions (what triggers defeat)
- Scoring system (points calculation)
- Economy (currency, earning, spending)
- Level progression (structure, difficulty)
- Difficulty settings and multipliers

**Output**: `gdd-flows.md`

**Diagram Format**:
Uses Mermaid diagrams for state machines and flows

---

### reversa-gdd-composer (Synthesis)

**Role**: Final documentation generation
**Size**: ~10.7 KB
**Synthesizes**:
- Integrates all 4 previous analyses
- Writes executive summary
- Creates professional GDD structure
- Adds confidence annotations
- Documents gaps
- Lists open questions
- Generates citations and references

**Output**: `game-design-document.md`

**Structure** (7 main sections):
1. Visão Geral (Overview)
2. Fluxo e Progressão (Flow & Progression)
3. Mecânicas Principais (Core Mechanics)
4. Entidades do Jogo (Game Entities)
5. Progressão e Dificuldade (Progression)
6. Análise Técnica (Technical Analysis)
7. Rastreabilidade (Traceability & Confidence)

## Confidence Levels

### 🟢 CONFIRMED
- **Source**: Direct extraction from code
- **Evidence**: File and line number citation
- **Reliability**: Very high (if code is correct)
- **Example**: `Game loop found: Update() at PlayerController.cs:45`

### 🟡 INFERRED
- **Source**: Pattern recognition and deduction
- **Evidence**: Pattern description and similar code locations
- **Reliability**: Medium (pattern may be incorrect)
- **Example**: `Possible double-jump: Loop condition detected but not explicitly coded`

### 🔴 GAP
- **Source**: Code review resulted in uncertainty
- **Evidence**: None found
- **Reliability**: Unknown (needs human validation)
- **Example**: `System of save not detected - requires human clarification`

## Integration with Reversa Framework

### State Management
```json
{
  "gdd_phase": "analyst",
  "gdd_completed_phases": ["scout"],
  "gdd_last_update": "2024-05-19T12:00:00Z"
}
```

### Output Folder Structure
```
_reversa_sdd/                           # Configurable output folder
├── gdd/                                # All GDD artefacts
│   ├── gdd-surface.json                # From Scout
│   ├── gdd-mechanics.md                # From Analyst
│   ├── gdd-entities.md                 # From Entities
│   └── gdd-flows.md                    # From Flows
└── game-design-document.md             # From Composer (FINAL)
```

### Config Respect
- `doc_level`: Affects detail level (essential/complete/detailed)
- `doc_language`: Generates output in configured language
- `output_folder`: All writes go here (default: `_reversa_sdd`)
- Non-destructive: Never modifies existing project files

## Engine-Specific Patterns

### Unity (C#)
**Detection**:
- Look for `using UnityEngine;`
- Find `MonoBehaviour` base class
- Search for `Update()`, `FixedUpdate()`, `Awake()`, `Start()`
- Check for `Assets/`, `ProjectSettings/` folders

**Physics Extraction**:
- `Rigidbody2D.velocity`, `Rigidbody.velocity`
- `Physics2D.gravity`, `Physics.gravity`
- `Animator` component for animations
- `OnCollisionEnter()`, `OnTriggerEnter()` for collisions

### Godot (GDScript)
**Detection**:
- Look for `extends Node`, `extends Node2D`, `extends Node3D`
- Find `_ready()`, `_process()`, `_physics_process()`
- Check for `.tscn` scene files
- Look for `project.godot` in root

**Physics Extraction**:
- `velocity` attribute assignments
- `get_physics_process_delta_time()`
- `Area2D`, `CharacterBody2D` node types
- `_on_*_entered()` signals

### Unreal Engine (C++)
**Detection**:
- Look for `#include "Engine.h"`
- Find `AActor`, `ACharacter` base classes
- Search for `Tick()` method
- Check for `*.uproject` file

**Physics Extraction**:
- `GetActorVelocity()`
- `GetCharacterMovement()`
- Physics component values in constructors
- `OnComponentBeginOverlap()` callbacks

## Output Format

### GDD Document Structure

```markdown
# Game Design Document: [Game Name]

[Header and metadata]

## Executive Summary
[3-4 paragraph overview]

## 1. Visão Geral (Game Concept & Player)
### 1.1 Conceito do Jogo
### 1.2 Gênero e Classificação
### 1.3 Público-Alvo
### 1.4 Tecnologia Base

## 2. Fluxo e Progressão
### 2.1 Arquitetura de Estados
### 2.2 Game Loop Principal
### 2.3 Condições de Vitória
### 2.4 Condições de Derrota
### 2.5 Economia

## 3. Mecânicas Principais
### 3.1 Controles
### 3.2 Física
### 3.3 Colisões
### 3.4 IA

## 4. Entidades
### 4.1 Player
### 4.2 Inimigos
### 4.3 NPCs
### 4.4 Itens

## 5. Progressão
### 5.1 Níveis
### 5.2 Dificuldade

## 6. Análise Técnica
### 6.1 Arquitetura
### 6.2 Dados
### 6.3 Performance

## 7. Rastreabilidade
### 7.1 Confiança
### 7.2 Gaps
### 7.3 Questões Abertas

## Apêndices
### A. Glossário
### B. Referências
### C. Diagramas
```

## Usage Flows

### Flow 1: Automatic Full Analysis
```
/reversa-gdd
  → Choose "Completo"
  → Orchestrator runs all 5 agents sequentially
  → Output: Complete GDD
```

### Flow 2: Manual Step-by-Step
```
/reversa-gdd-scout → Review → CONTINUAR
/reversa-gdd-analyst → Review → CONTINUAR
/reversa-gdd-entities → Review → CONTINUAR
/reversa-gdd-flows → Review → CONTINUAR
/reversa-gdd-composer → Final GDD
```

### Flow 3: Resume After Interruption
```
/reversa-gdd
  → Detects existing state.json
  → Asks: Continue from [last phase]? (Y/N)
  → Resumes from checkpoint
```

## Future Extensions

Possible future skills:

1. **reversa-gdd-reviewer** — AI review of gaps
2. **reversa-gdd-exporter** — Export to Confluence/Notion
3. **reversa-gdd-visualizer** — Interactive diagrams
4. **reversa-gdd-comparison** — Before/after analysis
5. **reversa-gdd-metrics** — Complexity scoring

## Limitations

### Cannot Detect:
- ❌ Game story/narrative (unless in code)
- ❌ Artistic intent or visual style
- ❌ Audio design details
- ❌ User acquisition strategy
- ❌ Monetization model
- ❌ Marketing positioning

### May Miss:
- Code commented out (as 🔴 GAP)
- Very abstracted architectures
- Indirect dependencies
- Dynamic behavior not visible in static code

## Testing Strategy

Skills were designed to work with:
- ✅ Existing games of various engines
- ✅ Sample game projects
- ✅ Custom/unknown engines (degraded mode)
- ✅ Partial game code (generates gaps)

User should test with:
1. Small game (10-20 scripts) — Quick validation
2. Medium game (50-100 scripts) — Normal use
3. Large game (1000+ scripts) — Performance test

## Performance Notes

Estimated runtimes:
- Scout: 2-3 minutes (folder scanning)
- Analyst: 5-8 minutes (code analysis)
- Entities: 4-6 minutes (parsing)
- Flows: 3-5 minutes (state mapping)
- Composer: 2-4 minutes (synthesis)

**Total**: 16-26 minutes for full analysis

Factors affecting speed:
- Project size (lines of code)
- Code complexity (FSMs, nested logic)
- Number of entities (enemies, NPCs, items)
- Engine/framework familiarity

## Documentation

### For Users:
- `GDD-QUICK-START.md` — Quick reference
- `docs/agentes/gdd-reverse-engineering.md` — Full English docs
- `docs/agentes/gdd-engenharia-reversa.pt.md` — Full Portuguese docs
- Each skill's `SKILL.md` — Detailed instructions

### For Developers:
- This file (`IMPLEMENTATION-NOTES.md`)
- Individual skill SKILL.md files
- Code comments in each SKILL.md

## Maintenance

### Adding New Engine Support
1. Add pattern detection to Scout
2. Add physics extraction patterns to Analyst
3. Document new patterns in relevant SKILL.md

### Improving Accuracy
1. Refine pattern matching in Scout
2. Add confidence heuristics in Analyst
3. Improve gap identification in Composer

### Scaling for Large Projects
1. Consider parallel agent execution
2. Add incremental analysis option
3. Implement caching for re-runs

---

**Created**: May 2024
**Framework**: Reversa (Specification Reverse-Engineering)
**License**: MIT
**Author**: sandeco + reversaGameDev contributors
