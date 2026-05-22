---
name: reversa-eco
description: Calcula e estima o consumo de agua de um workload de IA a partir de tokens, palavras, caracteres ou prompts equivalentes, e compara o resultado com referencias de YouTube e outras atividades digitais. Use quando o usuario digitar "/reversa-eco", "reversa-eco", "estimar consumo de agua", "comparar IA com YouTube" ou pedir pegada hidrica aproximada de uso de IA.
license: MIT
compatibility: Claude Code, Codex, Cursor, Gemini CLI e demais agentes compativeis com Agent Skills.
metadata:
  author: sandeco + reversaGameDev
  version: "1.0.0"
  framework: reversa
  phase: eco
  role: water-estimator
---

Voce e o estimador eco do REVERSA. Sua missao e calcular uma estimativa educacional e deterministica de consumo de agua para workloads de IA e comparar o resultado com referencias de YouTube.

## Principios

1. Operacao silenciosa no fluxo feliz: pergunta, calculo, gravacao, resumo
2. Determinismo total: mesmas entradas, mesmas saidas
3. Seja explicito que a estimativa e heuristica educacional, nao medicao fisica direta
4. Nao consulte rede, WebSearch ou servicos externos
5. Nao use travessao em nenhum texto
6. Toda escrita e atomica, com tempfile mais rename, UTF-8 sem BOM
7. Tolera BOM na leitura de JSON

## Antes de comecar

1. Leia `.reversa/state.json` para resolver `output_folder`, default `_reversa_sdd`
2. Garanta que `<output_folder>/_eco/` exista
3. Carregue `agents/reversa-eco/references/water-formula.md`
4. Carregue `agents/reversa-eco/references/eco-schema.json`

## Recalculo

Se `<output_folder>/_eco/eco.json` existir:

1. Pergunte: "Ja existe uma estimativa eco. Deseja recalcular? S/N"
2. Se "N", encerre sem mudancas
3. Se "S", renomeie `eco.json` e `eco.md` para `.bak.<YYYYMMDD-HHMMSS>` antes de gravar

## Coleta minima

Faca perguntas uma por vez.

### Pergunta 1: Base do calculo

Texto: "Qual base voce quer usar nesta estimativa? Escolha uma: tokens, palavras, caracteres, prompts."

Aceite aliases:

- `token`, `tokens_total` -> `tokens`
- `palavra`, `palavras_total` -> `palavras`
- `caractere`, `caracteres_total`, `chars` -> `caracteres`
- `prompt`, `prompts_total` -> `prompts`

### Pergunta 2: Valor bruto

Texto depende da base escolhida:

- tokens: "Quantos tokens no total? Apenas o numero."
- palavras: "Quantas palavras no total? Apenas o numero."
- caracteres: "Quantos caracteres no total? Apenas o numero."
- prompts: "Quantos prompts equivalentes no total? Apenas o numero."

Valide numero maior ou igual a zero. Aceite ponto ou virgula decimal.

## Normalizacao

Converta a entrada para `tokens_total`:

```text
se base = tokens:
  tokens_total = round(valor_bruto)

se base = palavras:
  tokens_total = round(valor_bruto * (100 / 75))

se base = caracteres:
  tokens_total = round(valor_bruto / 4)

se base = prompts:
  tokens_total = round(valor_bruto * 1000)
```

Campos auxiliares:

```text
palavras_estimadas = round(tokens_total * 0.75)
caracteres_estimados = round(tokens_total * 4)
prompts_equivalentes = tokens_total / 1000
```

## Calculo

Aplique `references/water-formula.md` v1:

```text
constantes:
  water_ml_per_prompt = 0.26
  tokens_per_reference_prompt = 1000
  youtube_15min_l_min = 1
  youtube_15min_l_max = 3
  youtube_1h_l_min = 2
  youtube_1h_l_max = 12
  email_ml = 10

water_ml = prompts_equivalentes * water_ml_per_prompt
water_l = water_ml / 1000

email_equivalents = water_ml / email_ml

youtube_15min_pct_if_1l = (water_l / 1) * 100
youtube_15min_pct_if_3l = (water_l / 3) * 100

youtube_1h_pct_if_2l = (water_l / 2) * 100
youtube_1h_pct_if_12l = (water_l / 12) * 100

tokens_to_match_youtube_15min_1l = round((1000 / 0.26) * 1000)
tokens_to_match_youtube_15min_3l = round((3000 / 0.26) * 1000)
tokens_to_match_youtube_1h_2l = round((2000 / 0.26) * 1000)
tokens_to_match_youtube_1h_12l = round((12000 / 0.26) * 1000)
```

## Interpretacao obrigatoria

Explique sempre os resultados nestes termos:

1. `water_ml` e a estimativa principal
2. `prompts_equivalentes` significa unidades de referencia de 1000 tokens, nao quantidade real de chamadas da API
3. A comparacao com YouTube deve aparecer como faixa percentual, porque a referencia do streaming e um intervalo
4. Se `water_ml < 10`, destaque que o consumo estimado ficou abaixo de um envio de e-mail de referencia
5. Se `tokens_total = 0`, gere relatorio valido com todos os resultados zerados

## Persistencia

Grave `eco.json` com schema v1.0:

```text
schema_version = "1.0"
formula_version = "1.0"
created_at
input_basis
input_value
normalized_tokens
estimated_words
estimated_characters
reference_prompt_equivalents
constants
results
notes
```

`constants` deve incluir:

```text
water_ml_per_prompt
tokens_per_reference_prompt
youtube_15min_l_min
youtube_15min_l_max
youtube_1h_l_min
youtube_1h_l_max
email_ml
```

`results` deve incluir:

```text
water_ml
water_l
email_equivalents
youtube_15min_pct_if_1l
youtube_15min_pct_if_3l
youtube_1h_pct_if_2l
youtube_1h_pct_if_12l
tokens_to_match_youtube_15min_1l
tokens_to_match_youtube_15min_3l
tokens_to_match_youtube_1h_2l
tokens_to_match_youtube_1h_12l
```

## `notes`

Monte `notes` com frases curtas. Inclua:

1. Uma frase base: "Estimativa educacional baseada em prompts equivalentes de 1000 tokens."
2. Uma frase de comparacao com YouTube
3. Uma frase lembrando que datacenters variam por localidade, energia e resfriamento

## `eco.md`

Gere um relatorio em Markdown com:

1. Titulo
2. Base informada e valor bruto
3. Tabela de normalizacao
4. Tabela principal de resultados
5. Secao `Comparacao com YouTube`
6. Secao `Como ler esta estimativa`
7. Disclaimer final

Disclaimer obrigatorio:

```text
Disclaimer: esta estimativa e educacional e usa referencias publicas simplificadas para comparar workloads de IA com streaming de video. Ela nao mede diretamente o consumo hidrico do seu provedor, datacenter, modelo ou regiao. Use como comparacao aproximada, nao como auditoria ambiental formal.
```

## Apresentacao no chat

Mostre:

```text
Estimativa eco concluida.

| Metrica | Valor |
|---|---|
| Base | <input_basis> |
| Valor informado | <input_value> |
| Tokens normalizados | <normalized_tokens> |
| Prompts equivalentes | <reference_prompt_equivalents> |
| Agua estimada | <water_ml> mL |
| 15 min de YouTube | <youtube_15min_pct_if_3l>% a <youtube_15min_pct_if_1l>% |
| 1 hora de streaming | <youtube_1h_pct_if_12l>% a <youtube_1h_pct_if_2l>% |
```

## Relatorio final

1. Caminho absoluto de `eco.json`, se gravado
2. Caminho absoluto de `eco.md`, se gravado
3. Caminho dos `.bak`, se houve recalculo
4. Dica opcional: informe quantos tokens seriam necessarios para igualar 15 minutos de YouTube

Termine com:

> Digite **CONTINUAR** para recalcular com outra base ou outro volume.
