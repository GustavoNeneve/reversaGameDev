# Water Formula v1.0

Esta formula produz uma estimativa educacional, deterministicamente reproduzivel, para comparar workloads de IA com referencias de YouTube.

## Hipoteses adotadas

- 1 prompt de referencia = 1000 tokens
- 1 prompt de referencia = 0,26 mL de agua
- 100 tokens = 75 palavras
- 1 token = 4 caracteres
- 15 minutos de YouTube = 1 a 3 litros
- 1 hora de streaming = 2 a 12 litros
- 1 envio de e-mail = 10 mL

## Conversoes

```text
palavras -> tokens = palavras * (100 / 75)
caracteres -> tokens = caracteres / 4
prompts -> tokens = prompts * 1000
```

## Formula principal

```text
prompts_equivalentes = tokens_total / 1000
water_ml = prompts_equivalentes * 0.26
water_l = water_ml / 1000
email_equivalents = water_ml / 10
```

## Comparacao com YouTube

```text
youtube_15min_pct_if_1l = (water_l / 1) * 100
youtube_15min_pct_if_3l = (water_l / 3) * 100

youtube_1h_pct_if_2l = (water_l / 2) * 100
youtube_1h_pct_if_12l = (water_l / 12) * 100
```

## Tokens necessarios para equiparar referencias

```text
tokens_to_match_youtube_15min_1l = round((1000 / 0.26) * 1000)
tokens_to_match_youtube_15min_3l = round((3000 / 0.26) * 1000)
tokens_to_match_youtube_1h_2l = round((2000 / 0.26) * 1000)
tokens_to_match_youtube_1h_12l = round((12000 / 0.26) * 1000)
```

## Leitura correta

- O resultado representa equivalencia por referencia, nao telemetria do provedor
- O intervalo do YouTube gera intervalo percentual, nao numero unico
- O uso real pode variar por modelo, data center, mix energetico e temperatura local
