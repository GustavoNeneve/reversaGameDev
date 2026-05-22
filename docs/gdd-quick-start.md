# GDD Quick Start

Use the modular `reversa-gdd` suite to reverse engineer game source code and generate a complete Game Design Document.

## 1) Install Reversa with GDD agents

```bash
npx reversa install --agent gdd
```

## 2) Run the orchestrator

```bash
/reversa-gdd
```

## 3) Choose execution mode

- **Complete**: full pipeline (`scout → analyst → entities → flows → composer`)
- **Fast**: reconnaissance + core analysis
- **Custom**: pick specific phases

## 4) Get outputs

```text
_reversa_sdd/
├── gdd/gdd-surface.json
├── gdd/gdd-mechanics.md
├── gdd/gdd-entities.md
├── gdd/gdd-flows.md
└── game-design-document.md
```

## Related pages

- [GDD Reverse Engineering suite](agentes/gdd-reverse-engineering.md)
- [Agents overview](agentes/index.md)
