# GDD Começo Rápido

Use a suíte modular `reversa-gdd` para fazer engenharia reversa de código-fonte de jogos e gerar um Game Design Document completo.

## 1) Instale o Reversa com os agentes de GDD

```bash
npx reversa install --agent gdd
```

## 2) Execute o orquestrador

```bash
/reversa-gdd
```

## 3) Escolha o modo de execução

- **Completo**: pipeline total (`scout → analyst → entities → flows → composer`)
- **Rápido**: reconhecimento + análise principal
- **Personalizado**: escolha fases específicas

## 4) Veja os artefatos gerados

```text
_reversa_sdd/
├── gdd/gdd-surface.json
├── gdd/gdd-mechanics.md
├── gdd/gdd-entities.md
├── gdd/gdd-flows.md
└── game-design-document.md
```

## Páginas relacionadas

- [Suíte GDD Reverse Engineering](agentes/gdd-reverse-engineering.md)
- [Visão geral dos agentes](agentes/index.md)

