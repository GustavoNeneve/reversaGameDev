# Game Design Document Reverse Engineering Skills

## Overview

O projeto **reversaGameDev** agora inclui um conjunto completo de skills especializadas para **Engenharia Reversa de Código de Jogo e Geração de Game Design Document (GDD)**.

Estas skills permitem que você analise o código-fonte de um jogo existente e gere um Game Design Document profissional, estruturado e rastreável automaticamente.

## O Problema que Resolve

A maioria dos jogos existentes tem seu design enterrado no código:
- Mecânicas undocumentadas
- Lógica de IA implícita
- Fluxos de jogo não-formalizados
- Economia de pontos não registrada
- Arquitetura de software inexplícita

As skills de GDD Reverse Engineering extraem essa informação dispersa, organizam-na e produzem um documento executivo completo com confiança e rastreabilidade.

## Skills Disponíveis

### 1. **reversa-gdd** (Orchestrator)
**Descrição**: Orquestrador central que coordena toda a pipeline de GDD.

**Ativa com**: `/reversa-gdd`, `reversa-gdd`, `game-design-document`, `gdd`

**Responsabilidades**:
- Gerencia checkpoint e retomada entre sessões
- Coordena sub-agentes especializados
- Pergunta modo de execução (completo/rápido/personalizado)
- Salva progresso em `.reversa/state.json`
- Apresenta relatório final

**Output principal**: Coordenação e state management

---

### 2. **reversa-gdd-scout** (Reconnaissance)
**Descrição**: Mapeia a superfície do código do jogo: estrutura de pastas, engine, linguagens, entry points.

**Ativa com**: `/reversa-gdd-scout`, `reversa-gdd-scout`, `game-scout`

**Responsabilidades**:
- Detecta estrutura de pastas do jogo
- Identifica engine/framework (Unity, Godot, Unreal, custom)
- Reconhece linguagens primárias
- Localiza entry points (main, GameManager, scenes)
- Mapeia dependências

**Output**: `gdd/gdd-surface.json`

**Exemplo de Achados**:
```json
{
  "engine": "Unity 2021 LTS",
  "primary_language": "C#",
  "inferred_game_type": "2D Platformer",
  "entry_points": ["MainMenu.unity", "GameManager.cs"]
}
```

---

### 3. **reversa-gdd-analyst** (Deep Analysis)
**Descrição**: Analisa em profundidade: game loop, inputs, física, colisões, estados, IA.

**Ativa com**: `/reversa-gdd-analyst`, `reversa-gdd-analyst`

**Responsabilidades**:
- Extrai e documenta o game loop (Update, FixedUpdate, etc.)
- Mapeia todos os controles de entrada
- Extrai valores de física (velocidade, gravidade, aceleração)
- Documenta sistemas de colisão
- Mapeia máquinas de estado (FSM)
- Analisa inteligência artificial de inimigos

**Output**: `gdd/gdd-mechanics.md`

**Exemplo de Achados**:
```markdown
## Game Loop
🟢 CONFIRMED: `GameManager.cs:45`
- Update() a 60 FPS
- FixedUpdate() para física

## Controles
| Ação | Input | Linha |
|------|-------|-------|
| Mover | A/D | PlayerController.cs:67 |
| Pular | Space | PlayerController.cs:42 |
```

---

### 4. **reversa-gdd-entities** (Entity Analysis)
**Descrição**: Analisa entidades do jogo: Player, Inimigos, NPCs, Itens.

**Ativa com**: `/reversa-gdd-entities`, `reversa-gdd-entities`

**Responsabilidades**:
- Extrai atributos completos do player (HP, speed, etc.)
- Documenta todos os tipos de inimigos
- Mapeia máquinas de estado por inimigo
- Identifica NPCs e suas interações
- Cataloga itens e coletáveis
- Documenta efeitos e comportamentos

**Output**: `gdd/gdd-entities.md`

**Exemplo de Achados**:
```markdown
## Player
🟢 Max Health: 100 HP | LineNumber: 12
🟢 Speed: 5 unidades/seg | LineNumber: 14

## Inimigo: Walker
- States: Patrol, Chase, Attack, Die
- HP: 20
- Detection Range: 8 unidades
```

---

### 5. **reversa-gdd-flows** (Flow & Progression)
**Descrição**: Analisa fluxos de jogo, progressão, economia e condições de vitória/derrota.

**Ativa com**: `/reversa-gdd-flows`, `reversa-gdd-flows`

**Responsabilidades**:
- Mapeia transições de estado do jogo (Menu → Playing → GameOver)
- Identifica condições de vitória
- Identifica condições de derrota
- Documenta sistema de moeda/pontos
- Mapeia progressão de níveis
- Analisa dificuldade e suas variações

**Output**: `gdd/gdd-flows.md`

**Exemplo de Achados**:
```markdown
## Game Flow
[Diagrama Mermaid com estados]

## Condições de Vitória
🟢 CONFIRMED: Derrotar 5 inimigos

## Economia
Coins: Ganhos de inimigos (5-20), Gastos na loja (50-500)
```

---

### 6. **reversa-gdd-composer** (Synthesis)
**Descrição**: Sintetiza todas as análises em um Game Design Document profissional e completo.

**Ativa com**: `/reversa-gdd-composer`, `reversa-gdd-composer`

**Responsabilidades**:
- Integra todos os artefatos anteriores
- Cria GDD estruturado e executivo
- Adiciona Executive Summary
- Marca confiança (🟢🟡🔴) em cada achado
- Gera seções de gaps e questões abertas
- Formata para apresentação profissional

**Output**: `game-design-document.md` (arquivo final para compartilhar)

**Estrutura do GDD Final**:
```
# Game Design Document: [Game Name]

## Executive Summary

## 1. Visão Geral
## 2. Fluxo e Progressão
## 3. Mecânicas Principais
## 4. Entidades do Jogo
## 5. Progressão e Dificuldade
## 6. Análise Técnica
## 7. Rastreabilidade e Confiança

## Apêndices
```

---

## Como Usar

### Fluxo Básico

1. **Inicie o Orchestrator**:
   ```
   /reversa-gdd
   ```

2. **Escolha modo** (completo, rápido, personalizado)

3. **Aguarde execução dos sub-agentes** em sequência

4. **Revise cada artefato** antes de prosseguir

5. **Obtenha o GDD final** em `game-design-document.md`

### Fluxo Manual (Controlado)

Você pode invocar cada skill individualmente:

```
/reversa-gdd-scout         # Mapear superfície
/reversa-gdd-analyst       # Analisar mecânicas
/reversa-gdd-entities      # Analisar entidades
/reversa-gdd-flows         # Analisar fluxos
/reversa-gdd-composer      # Sintetizar GDD
```

## Estrutura de Output

```
_reversa_sdd/
├── gdd/
│   ├── gdd-surface.json      # Detectado pelo Scout
│   ├── gdd-mechanics.md      # Análise do Analyst
│   ├── gdd-entities.md       # Análise do Entities
│   └── gdd-flows.md          # Análise do Flows
└── game-design-document.md   # GDD final (Composer)
```

## Escala de Confiança

Todos os achados são marcados com confiança:

| Marca | Significado | Exemplo |
|-------|-----------|---------|
| 🟢 CONFIRMED | Extraído diretamente do código, com citação | `PlayerController.cs:42` |
| 🟡 INFERRED | Deduzido de padrões, pode estar errado | "Possível double-jump (código comentado)" |
| 🔴 GAP | Não determinável do código, requer validação | "Sistema de save não detectado" |

## Suporte a Engines

As skills foram desenhadas para funcionar com qualquer engine/framework:

- ✅ **Unity** (C#) — Detecta MonoBehaviour, Update, Physics
- ✅ **Godot** (GDScript) — Detecta _ready(), _process(), Areas
- ✅ **Unreal** (C++) — Detecta AActor, GameMode, Movement
- ✅ **Custom Engines** (C++, Python, etc.) — Analisa estrutura genérica
- ✅ **Web Games** (JavaScript/TypeScript) — Detecção de bibliotecas (Phaser, Three.js)

## Integração com Reversa Core

As skills de GDD se integram ao ecossistema Reversa:

- ✅ Respeitam `.reversa/state.json`
- ✅ Usam `output_folder` configurável
- ✅ Suportam `doc_level` (essencial, completo, detalhado)
- ✅ Preservam original do projeto (jamais modifica)
- ✅ Salvas em `.agents/skills/reversa-gdd*/`

## Casos de Uso

### 1. **Documentação de Legacy Game**
Seu jogo foi desenvolvido sem documentação. Use GDD para:
- Mapear o que existe
- Criar especificação retroativa
- Facilitar onboarding de novos desenvolvedores

### 2. **Preparar para Refatoração**
Antes de refatorar, execute GDD para:
- Entender todos os fluxos
- Identificar dependências
- Gerar baseline de testes

### 3. **Análise Técnica de Projeto Existente**
Avaliar viabilidade de:
- Port para outra engine
- Adição de multiplayer
- Expansão de conteúdo

### 4. **Design Review**
Validar decisões de design:
- Mecânicas bem implementadas?
- Economia balanceada?
- Curva de dificuldade apropriada?

## Exemplo Prático

```
Projeto: MyPlatformer (Unity 2D, C#)

1. /reversa-gdd                    # Inicia orchestrator
   ↓
2. Escolhe: Análise Completa       # 20+ minutos
   ↓
3. /reversa-gdd-scout              # 2 minutos
   ✅ Detecta: Unity 2021, C#, 2D platformer
   ↓
4. /reversa-gdd-analyst            # 5 minutos
   ✅ Extrai: Game loop, 5 controles, physics, 2 inimigos
   ↓
5. /reversa-gdd-entities           # 4 minutos
   ✅ Documenta: Player (100 HP, 5 speed), Walkers, Items
   ↓
6. /reversa-gdd-flows              # 4 minutos
   ✅ Mapeia: 5 níveis, 3 dificuldades, economia coins/xp
   ↓
7. /reversa-gdd-composer           # 3 minutos
   ✅ Gera: game-design-document.md completo

RESULTADO:
📄 game-design-document.md (profissional, rastreável, pronto para apresentar)
```

## Próximas Extensões Possíveis

- **reversa-gdd-reviewer**: Revisor que valida gaps com questões
- **reversa-gdd-exporter**: Exporta GDD para Confluence, Notion, Google Docs
- **reversa-gdd-visualizer**: Cria diagramas visuais interativos
- **reversa-gdd-comparison**: Compara 2 GDDs (antes/depois refatoração)
- **reversa-gdd-metrics**: Calcula métricas de complexidade do jogo

## Troubleshooting

### "Scout não encontrou entry point"
- Verifique se o projeto tem estrutura clara
- Procure por `main.cs`, `GameManager.cs`, `main.tscn`, etc.
- Se usar engine customizada, especifique caminho manualmente

### "Analyst não encontra game loop"
- Verifique se existe `Update()`, `_process()`, ou similar
- Se engine customizada, procure por while loops em `main()`

### "Não consigo ver o arquivo final"
- Output está em `_reversa_sdd/game-design-document.md` (padrão)
- Verifique com `ls -la _reversa_sdd/`

## Documentação Completa

Cada skill tem seu próprio `SKILL.md` detalhado:
- `agents/reversa-gdd/SKILL.md` — Orchestrator
- `agents/reversa-gdd-scout/SKILL.md` — Scout
- `agents/reversa-gdd-analyst/SKILL.md` — Analyst
- `agents/reversa-gdd-entities/SKILL.md` — Entities
- `agents/reversa-gdd-flows/SKILL.md` — Flows
- `agents/reversa-gdd-composer/SKILL.md` — Composer

---

**reversa-gdd** — Transforme seu código de jogo em design document profissional. 👾📄
