# Skills de Engenharia Reversa para Game Design Document (GDD)

## Visão geral

A suíte **reversa-gdd** analisa código-fonte de jogos e gera um **Game Design Document (GDD)** estruturado com base em evidências do código.

## Problema que resolve

Em muitos projetos de jogos, decisões de design ficam implícitas no código:

- Mecânicas sem documentação
- IA e transições de estado pouco explícitas
- Regras de pontuação/economia difíceis de auditar
- Documentação técnica incompleta

O Reversa reconstrói esses elementos em uma trilha documental rastreável.

## Agentes da suíte

### 1. `reversa-gdd` (coordenador)
- Orquestra o pipeline completo de GDD
- Gerencia checkpoints e retomada
- Suporta execução completa, rápida e personalizada

### 2. `reversa-gdd-scout`
- Reconhecimento do repositório e da stack de jogo
- Detecção de engine/framework/linguagens
- Identificação de entry points

### 3. `reversa-gdd-analyst`
- Extração de game loop e inputs
- Análise de física, colisões e lógica de IA

### 4. `reversa-gdd-entities`
- Documenta player, inimigos, NPCs e itens
- Mapeia atributos, estados e interações

### 5. `reversa-gdd-flows`
- Mapeia transições de telas/estados
- Extrai condições de vitória/derrota
- Documenta progressão e economia

### 6. `reversa-gdd-composer`
- Compõe o GDD final em Markdown
- Consolida artefatos intermediários
- Preserva marcações de confiança

## Fluxo recomendado

```text
/reversa-gdd
  └─ scout → analyst → entities → flows → composer
```

Também é possível executar cada agente separadamente:

```text
/reversa-gdd-scout
/reversa-gdd-analyst
/reversa-gdd-entities
/reversa-gdd-flows
/reversa-gdd-composer
```

## Estrutura de saída

```text
_reversa_sdd/
├── gdd/gdd-surface.json
├── gdd/gdd-mechanics.md
├── gdd/gdd-entities.md
├── gdd/gdd-flows.md
└── game-design-document.md
```

## Escala de confiança

| Marca | Significado |
|------|-------------|
| 🟢 CONFIRMED | Extraído diretamente do código com citação |
| 🟡 INFERRED | Dedução por padrão |
| 🔴 GAP | Não determinável com o código disponível |

## Início rápido

```bash
npx reversa install --agent gdd
```

Depois execute:

```text
/reversa-gdd
```

Veja também: [GDD Início Rápido](../gdd-quick-start.md)

