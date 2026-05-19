# Inicio Rápido GDD

Usa la suite modular `reversa-gdd` para hacer ingeniería inversa de código fuente de juegos y generar un Game Design Document completo.

## 1) Instala Reversa con los agentes GDD

```bash
npx reversa install --agent gdd
```

## 2) Ejecuta el orquestador

```bash
/reversa-gdd
```

## 3) Elige el modo de ejecución

- **Completo**: pipeline total (`scout → analyst → entities → flows → composer`)
- **Rápido**: reconocimiento + análisis principal
- **Personalizado**: elige fases específicas

## 4) Revisa los artefactos generados

```text
_reversa_sdd/
├── gdd/gdd-surface.json
├── gdd/gdd-mechanics.md
├── gdd/gdd-entities.md
├── gdd/gdd-flows.md
└── game-design-document.md
```

## Páginas relacionadas

- [Suite GDD Reverse Engineering](agentes/gdd-reverse-engineering.md)
- [Visión general de agentes](agentes/index.md)

