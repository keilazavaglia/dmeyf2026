# Undersampling: resultados de las tres corridas completas (Keila)

Corridas hechas del 15 al 18/11/2026 en la compu de Keila, con el código de la rama `feature/undersampling`
(commit `249aa01`, de Jaqueline) **sin cambios en el método**. Este archivo resume los resultados de las tres
opciones, los compara y explica cómo leerlos. Los resúmenes completos que genera el código están en las
subcarpetas `grid_per_tree/`, `optuna_per_tree/` y `grid_before/` (`summary.md`).

## 1. Qué se corrió

| Opción | Cómo se elige r | Cómo se aplica r | Comando |
|---|---|---|---|
| **1** | grid: una búsqueda de Optuna por cada r | en cada árbol (`pos/neg_bagging_fraction`) | `--r-mode grid --r-apply per_tree` |
| **2_excluido** | Optuna elige r de la lista, en una sola búsqueda | en cada árbol | `--r-mode optuna --r-apply per_tree` |
| **3_ahora_2** | grid: una búsqueda por cada r | **antes** de LightGBM: todas las bajas + r de los CONTINUA (sugerencia del tutor) | `--r-mode grid --r-apply before` |

**Numeración de la presentación:** el experimento quedó formado por las opciones 1 y 3 (que en las slides pasa a
llamarse **opción 2**). La opción 2 original (Optuna elige r) quedó **excluida** del experimento y se presenta
como extra: encuentra un r que funciona, pero no sirve para elegir r (ver secciones 4 y 5). Por eso se marcan
como `2_excluido` y `3_ahora_2`; los nombres de carpetas no cambian.

Configuración común (la misma que la corrida completa de Jaqueline, `shared_config.json`):

- Dataset de la competencia 2, lags y deltas 1 y 2 (760 variables). Train **201901–06**, validación **201908**, test **201910**.
- 15 valores de r (0,8 a 0,01) + baseline r = 1. **50 trials** por búsqueda, lr libre entre 0,01 y 0,3, early stopping 100.
- Elección de r en 201908 con 10 semillas (intervalo pareado 95%, margen δ = 1%); test del ganador (top 3 × 20 semillas) en 201910.
- Ganancia: la del código, contactando el % de clientes fijado en validación (~3%).
- Máquina: Windows 11, 12 núcleos, 15 GB, **8 hilos** en todas las corridas (los tiempos de las tres opciones son comparables entre sí).

La búsqueda baseline y sus entrenamientos los comparten las tres opciones (misma carpeta de resultados).

## 2. Resultado principal (test 201910, ganador vs baseline)

| Opción | r ganador | Ganancia: diferencia (IC 95%) | AUC: diferencia | Tiempo total | Entrenamiento final | Segundos por árbol |
|---|---|---|---|---|---|---|
| 1 grid · en cada árbol | **0,04** | +0,14 M [−1,40; +1,67] | +0,0007 | **−77,2%** | 212 → 46 s (−78%) | 0,43 → 0,055 |
| 2_excluido Optuna · en cada árbol | **0,1** | +1,12 M [−0,24; +2,48] | +0,0005 | **−67,8%** | 212 → 46 s (−78%) | 0,43 → 0,113 |
| 3_ahora_2 grid · **antes** | **0,1** | +0,34 M [−0,96; +1,68] | −0,0001 | **−87,5%** | 212 → 27 s (−87%) | 0,43 → 0,032 |

Baseline: ganancia 281,0 M, AUC 0,9367. "Tiempo total" = preparación + búsqueda + entrenamiento final + predicción
(la búsqueda del ganador en grid; toda la búsqueda en Optuna).

**En las tres opciones (incluida la excluida) se cumplen las dos hipótesis:** el tiempo baja mucho más del 30% pedido (H1) y la ganancia
y el AUC no caen más que el margen (H2). Recortar **antes** de LightGBM es lo que más ahorra.

## 3. Lectura por par de configuraciones (no solo el agregado)

El test compara las 3 mejores configuraciones de cada lado con 20 semillas. El resumen del código junta los 60
pares como si fueran independientes, lo que da intervalos algo optimistas (hay solo 3 comparaciones distintas,
cada una repetida con 20 semillas). Por eso conviene mirar cada par por separado:

| Opción | Par | Trial baseline / ganador | Ganancia (M) | IC 95% | Mejor en | AUC |
|---|---|---|---|---|---|---|
| 1 | 1º | 39 / 41 | +3,58 | [+0,88; +6,11] | 15/20 | +0,0007 |
| 1 | 2º | 33 / 48 | −2,37 | [−4,90; +0,49] | 6/20 | +0,0011 |
| 1 | 3º | 48 / 44 | −0,80 | [−2,67; +1,13] | 7/20 | +0,0002 |
| 2_excluido | 1º | 39 / 18 | +3,03 | [+0,11; +5,88] | 14/20 | −0,0006 |
| 2_excluido | 2º | 33 / 43 | +2,15 | [+0,66; +3,58] | 16/20 | +0,0012 |
| 2_excluido | 3º | 48 / 17 | −1,81 | [−3,63; −0,05] | 8/20 | +0,0009 |
| 3_ahora_2 | 1º | 39 / 6 | +2,61 | [−0,19; +5,25] | 14/20 | −0,0007 |
| 3_ahora_2 | 2º | 33 / 46 | +0,17 | [−1,81; +2,25] | 11/20 | +0,0006 |
| 3_ahora_2 | 3º | 48 / 37 | −1,76 | [−3,46; −0,11] | 6/20 | −0,0004 |

Lectura: la ganancia queda **aproximadamente igual** (entre −2,4 y +3,6 M según la configuración, sobre ~281 M);
no hay evidencia de que gane ni de que pierda de forma sistemática. Ninguna conclusión cambia, pero conviene
presentarla así y no con el p-valor agregado.

## 4. Curvas de r (test, mejor configuración de cada r)

- **Opción 3_ahora_2 (antes):** mantienen la ganancia los r de 0,8 a 0,025 (salvo 0,3 y 0,03, por poco); con **r ≤ 0,02 se
  pierde** (−5 a −8 M). El código marca la zona útil en **[0,025; 0,1]**. Coincide con lo que anticipó el tutor
  (~2%, "un poco más con pocos meses"): con r = 0,02 el dataset queda en ~15.000 filas.
- **Opción 1 (en cada árbol):** casi todos los r hasta **0,01** mantienen o mejoran la ganancia. Recortar en cada
  árbol tolera r más chicos (el modelo ve todos los CONTINUA a lo largo de los árboles), pero cada árbol ahorra menos.
  Zona útil marcada por el código: [0,01; 0,04].
- **Opción 2_excluido (Optuna elige r):** Optuna concentró 29 de 50 trials en r = 0,1; la mayoría de los otros r tuvo **un
  solo trial**. Su curva de r **no es informativa** (refleja ese único trial, no el efecto de r).

## 5. Comparación entre opciones

- **Antes vs en cada árbol (3_ahora_2 vs 1):** con r = 0,1, recortar antes ahorra mucho más (cada árbol 13 veces más barato
  vs ~8 veces) porque LightGBM trabaja sobre un dataset realmente chico. A cambio, con r muy chicos (≤ 0,02) pierde
  ganancia, y en cada árbol no.
- **r fijo vs elegido por Optuna (1 vs 2_excluido):** Optuna llegó solo a un r que funciona (0,1), pero no busca el r más
  chico porque no mira el tiempo, y su búsqueda no deja ver la curva de r. El grid es más caro (15 búsquedas) pero
  responde mejor la pregunta.
- **Costo total de explorar:** opción 1: 33,2 h; opción 2_excluido: 11,7 h; opción 3_ahora_2: 21,7 h (búsquedas + entrenamientos finales).
  En la vida real el r se busca una vez; lo que importa en producción es el entrenamiento del ganador (último
  cuadro de la sección 2).
- **El ahorro no viene del learning rate:** el lr elegido es parecido en baseline y ganadores (~0,012–0,015); el
  ahorro viene de que cada árbol cuesta mucho menos.

## 6. Limitaciones

- Un solo mes de validación y uno de test. En pruebas previas (simulando julio, agosto y septiembre) el efecto sobre
  la ganancia cambió de un mes a otro.
- Margen δ = 1%: es menor que el ruido entre semillas (~1,3%); se puede recalcular con otro δ sin volver a correr.
- Test top 3 × 20 agregado como 60 pares (ver sección 3).
- La ganancia depende de cómo se mide: con la franja 5–9% de contactos los resultados son algo peores para el
  undersampling que con el % fijado en validación.
- Train 201901–06 (la consigna dice 201903–06); en el modo "antes", la misma muestra de CONTINUA en todos los trials de
  una búsqueda; las bajas también se sortean con el `bagging_fraction` de LightGBM.
- Tiempos medidos en una sola máquina (comparables entre opciones, no con otras máquinas).

## 7. Reproducibilidad

- Las variables tienen el mismo `content_sha256` que las de la corrida de Jaqueline (mismos datos).
- El baseline da prácticamente lo mismo en las dos máquinas (test: 284,8 vs 284,7 M), aunque Optuna eligió otro
  trial ganador (33 vs 39) por diferencias numéricas entre Mac y Windows.
- Comandos (desde `undersampling/`, con `--config` apuntando al dataset local):

```bash
python main.py --name full_ksz --r-mode grid   --r-apply per_tree --n-trials 50 --threads 8
python main.py --name full_ksz --r-mode optuna --r-apply per_tree --n-trials 50 --threads 8
python main.py --name full_ksz --r-mode grid   --r-apply before   --n-trials 50 --threads 8
```

- En Windows hicieron falta dos ajustes que no cambian el método ni los tiempos: en `pipeline/utils.py`, `import
  resource` (no existe en Windows) se reemplazó por `psutil` para medir la memoria, y se corrió con `PYTHONUTF8=1`
  para que `summary.md` acepte la "δ".

## 8. Archivos

- `grid_per_tree/`, `optuna_per_tree/`, `grid_before/`: `summary.md`/`.json`, configuración, entorno y tiempos de cada opción.
- `trials/`: un CSV por búsqueda (31) con hiperparámetros, árboles, segundos por árbol y métricas de cada trial.
- `runs.csv`: cada entrenamiento final (configuración × semilla) con tiempos, AUC y ganancia.
- No se suben la base de Optuna, las predicciones ni los datos (pesados; el repo es público).
