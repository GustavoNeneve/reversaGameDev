# Estimador Eco de Agua

## Visión general

`/reversa-eco` estima la huella hídrica de un workload de IA a partir de **tokens, palabras, caracteres o prompts de referencia** y compara el resultado con **rangos de consumo de YouTube**.

## Qué resuelve

Este agente es útil cuando quieres una comparación ambiental simple, por ejemplo:

- cuánta agua representa aproximadamente este volumen de tokens
- cómo se compara esa estimación con 15 minutos de YouTube
- cuántos tokens harían falta para alcanzar el mismo rango hídrico que 1 hora de streaming

## Diferencia importante frente a pricing

`/reversa-pricing-size` **no** cuenta tokens ni LOC.

Si el objetivo es una **estimación de agua basada en tokens**, usa `/reversa-eco`.

## Entradas aceptadas

Puedes empezar desde una de estas bases:

- `tokens`
- `palavras`
- `caracteres`
- `prompts`

El agente normaliza todo a tokens usando estas conversiones de referencia:

- `100 tokens = 75 palabras`
- `1 token = 4 caracteres`
- `1 prompt de referencia = 1000 tokens`

## Fórmula principal

```text
prompts_equivalentes = tokens_total / 1000
water_ml = prompts_equivalentes * 0.26
water_l = water_ml / 1000
```

## Comparación con YouTube

El reporte compara el resultado con estos rangos:

- `15 min de YouTube = 1 a 3 L`
- `1 hora de streaming = 2 a 12 L`

Como YouTube aparece como un rango, la comparación también se muestra como rango.

## Estructura de salida

```text
_reversa_sdd/
└── _eco/
    ├── eco.json
    └── eco.md
```

## Puntos destacados de la salida

El reporte generado incluye:

- base informada y tokens normalizados
- palabras y caracteres estimados
- agua estimada en mL y L
- cantidad equivalente de prompts de referencia
- porcentaje frente a 15 minutos de YouTube
- porcentaje frente a 1 hora de streaming
- umbrales de tokens necesarios para igualar los rangos de YouTube

## Ejemplo

`1.000.000 tokens` se convierten en:

- `1000` prompts de referencia
- `260 mL` de agua estimada
- cerca de `8,67% a 26%` de `15 min` de YouTube

## Disclaimer

Esta es una estimación educativa. Sirve para comparación relativa, no para auditoría ambiental formal.
