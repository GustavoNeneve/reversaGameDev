# Skills de Ingeniería Inversa para Game Design Document (GDD)

## Visión general

El proyecto **Reversa** incluye una suite de agentes para analizar código fuente de juegos y producir un **Game Design Document (GDD)** estructurado, trazable y listo para revisión técnica.

## Problema que resuelve

En muchos juegos el diseño está implícito en el código:

- Mecánicas sin documentación
- Lógica de IA distribuida
- Estados y transiciones no formalizados
- Economía/puntuación difícil de auditar

La suite GDD de Reversa reconstruye ese conocimiento automáticamente y lo organiza en artefactos legibles.

## Agentes de la suite

### 1. `reversa-gdd` (coordinador)

- Orquesta la pipeline completa
- Gestiona checkpoints y reanudación por estado
- Permite modo completo, rápido o personalizado

### 2. `reversa-gdd-scout`

- Reconocimiento del repositorio de juego
- Detección de engine/framework/lenguajes
- Identificación de entry points

### 3. `reversa-gdd-analyst`

- Extracción de game loop
- Mapeo de inputs
- Física, colisiones y comportamiento de IA

### 4. `reversa-gdd-entities`

- Modelado de Player, enemigos, NPCs e ítems
- Estados, atributos y relaciones de entidades

### 5. `reversa-gdd-flows`

- Diagrama de estados y transiciones de pantallas
- Condiciones de victoria/derrota
- Economía y progresión

### 6. `reversa-gdd-composer`

- Compone el GDD final en Markdown
- Integra todos los artefactos intermedios
- Marca hallazgos por nivel de confianza

## Flujo recomendado

```text
/reversa-gdd
  └─ scout → analyst → entities → flows → composer
```

También puedes ejecutar cada agente por separado:

```text
/reversa-gdd-scout
/reversa-gdd-analyst
/reversa-gdd-entities
/reversa-gdd-flows
/reversa-gdd-composer
```

## Estructura de salida

```text
_reversa_sdd/
├── gdd/gdd-surface.json
├── gdd/gdd-mechanics.md
├── gdd/gdd-entities.md
├── gdd/gdd-flows.md
└── game-design-document.md
```

## Escala de confianza

| Marca | Significado |
|------|-------------|
| 🟢 CONFIRMED | Evidencia directa del código (con cita) |
| 🟡 INFERRED | Inferido por patrones |
| 🔴 GAP | No determinable con el código disponible |

## Compatibilidad

La suite fue diseñada para Unity, Godot, Unreal, engines personalizados y juegos web (JS/TS), adaptando la estrategia de análisis según la stack detectada.

## Inicio rápido

```bash
npx reversa install --agent gdd
```

Luego ejecuta:

```text
/reversa-gdd
```

Ver también: [Inicio Rápido GDD](../gdd-quick-start.md)
