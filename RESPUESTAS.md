# Respuestas Parcial 2

## P2. Fallas del Jenkinsfile

1. `bru run ... || true` en el stage `Regresion`: el `|| true` tragaba el código de salida de Bruno, así que el stage nunca fallaba y el pipeline quedaba en verde aunque la regresión estuviera en rojo (no era puerta).
2. `junit` leía `bruno/smoke/results.xml`, pero el reporte se genera en `bruno/regresion/results.xml`: Jenkins nunca publicaba los resultados de la regresión (y `allowEmptyResults: true` escondía que no había reporte).
3. `Build image` corría antes de `Regresion`: la imagen del API se construía aunque las pruebas no hubieran pasado, así que una versión rota llegaba a imagen; ahora se construye solo después de que la regresión pasa.

## P3. Builds del job `parcial2`

| Paso | Build | Resultado | SHA |
|---|---|---|---|
| 1. Jenkinsfile corregido | #1 | Verde | `16a8da185b6d18cec6e06371392d558d84031512` |
| 2. Prueba de P1 (tope de Q60) | #2 | Rojo | `94a415acaca52f4adab2ca9e8ffda05c3cdee8d3` |
| 3. `api.py` cumple el tope | #4 | Verde | `322b2d77f1b4e410fcb2c2a13781235e9b73b5ed` |

El build #3 lo disparó el polling sobre el mismo commit del paso 2 (`94a415a`), porque coincidió con el *Build Now* del #2. No es un paso aparte.

## Aserción que falló en el build rojo (#2)

`14-cobro-tope-siete-horas.res.body.monto eq 60`

En el mismo build también falló `16-cobro-tope-diez-horas.res.body.monto eq 60`.
