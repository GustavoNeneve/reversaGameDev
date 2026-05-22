# Estimador Eco de Água

## Visão geral

`/reversa-eco` estima a pegada hídrica de um workload de IA a partir de **tokens, palavras, caracteres ou prompts de referência** e compara o resultado com **faixas de consumo do YouTube**.

## O que ele resolve

Esse agente é útil quando você quer uma comparação ambiental simples, por exemplo:

- quanta água esse volume de tokens representa aproximadamente
- como essa estimativa se compara com 15 minutos de YouTube
- quantos tokens seriam necessários para atingir a mesma faixa hídrica de 1 hora de streaming

## Diferença importante em relação ao pricing

`/reversa-pricing-size` **não** conta tokens nem LOC.

Se o objetivo for **estimativa de água baseada em tokens**, use `/reversa-eco`.

## Entradas aceitas

Você pode começar por uma destas bases:

- `tokens`
- `palavras`
- `caracteres`
- `prompts`

O agente normaliza tudo para tokens usando estas conversões de referência:

- `100 tokens = 75 palavras`
- `1 token = 4 caracteres`
- `1 prompt de referência = 1000 tokens`

## Fórmula principal

```text
prompts_equivalentes = tokens_total / 1000
water_ml = prompts_equivalentes * 0.26
water_l = water_ml / 1000
```

## Comparação com YouTube

O relatório compara o resultado com estas faixas:

- `15 min de YouTube = 1 a 3 L`
- `1 hora de streaming = 2 a 12 L`

Como o YouTube aparece como faixa, a comparação também é mostrada como faixa.

## Estrutura de saída

```text
_reversa_sdd/
└── _eco/
    ├── eco.json
    └── eco.md
```

## Destaques da saída

O relatório gerado inclui:

- base informada e tokens normalizados
- palavras e caracteres estimados
- água estimada em mL e L
- quantidade equivalente de prompts de referência
- percentual frente a 15 minutos de YouTube
- percentual frente a 1 hora de streaming
- limiares de tokens necessários para igualar as faixas do YouTube

## Exemplo

`1.000.000 tokens` viram:

- `1000` prompts de referência
- `260 mL` de água estimada
- cerca de `8,67% a 26%` de `15 min` de YouTube

## Disclaimer

Esta é uma estimativa educacional. Ela serve para comparação relativa, não para auditoria ambiental formal.
