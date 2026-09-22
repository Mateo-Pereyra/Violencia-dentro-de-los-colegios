# Rompebrechas — Hito 2
### Registro de violencia escolar en colegios de primaria del Perú (2024–2025)

Este repositorio contiene el notebook `Rompebrechas_Hito2.ipynb`, que reproduce el proceso completo **desde las fuentes hasta la matriz analítica**: carga, diagnóstico, limpieza, integración, EDA, transformación de variables y construcción de la matriz analítica final.

## Entregables del Hito 2

| Entregable | Ubicación |
|---|---|
| Documento Word con el desarrollo y justificación de las decisiones | `docs/Rompebrechas_Hito2.docx` |
| Código fuente documentado (Python/Colab) | `Rompebrechas_Hito2.ipynb` |
| Repositorio GitHub actualizado (código, notebook, README, instrucciones) | este repositorio |
| Enlace al repositorio | `https://github.com/Mateo-Pereyra/Violencia-dentro-de-los-colegios` |


---

## Problema, usuario y objetivo

**Problema:** ¿Qué factores institucionales se asocian al **registro** de casos de violencia escolar en los colegios de educación primaria del Perú, y cómo se relaciona ese registro con la repetición y el retiro escolar, en el período 2024-2025?

**Usuario:** especialistas de convivencia escolar de las UGEL y DRE, y el equipo del MINEDU a cargo del SíseVe — quienes deciden en qué colegios capacitar, acompañar o supervisar el registro de casos.

**Beneficiarios:** los estudiantes de primaria, en especial los de colegios donde hoy la violencia no se está registrando y por lo tanto no se atiende.

**Objetivo de análisis:** construir una base a nivel colegio que permita (1) identificar qué características institucionales se asocian a que un colegio registre incidencias de violencia, y (2) evaluar su relación con la repetición y el retiro escolar.

**Unidad de análisis:** el servicio educativo de primaria (colegio), identificado por la llave compuesta `COD_MOD` + `ANEXO`.

---

## Cambios respecto al Hito 1

| Aspecto | Hito 1 | Hito 2 |
|---|---|---|
| Problema | Factores que influyen en los *casos* de violencia | Factores asociados al *registro* de violencia, y su relación con repetición y retiro |
| Fuente de violencia | Denuncias del portal SíseVe (nivel UGEL) | `P124B_SI` y `P126B_SI_1` del Censo Educativo (nivel colegio) |
| Fuentes nuevas | — | Resultado del Ejercicio 2025 y Matrícula 2025 |
| Unidad de análisis | Mezcla de colegio y denuncia | Colegio (`COD_MOD` + `ANEXO`) en las cuatro fuentes |

El cambio central fue abandonar el cruce con el portal SíseVe a nivel UGEL (que producía una falacia ecológica al analizarlo como si fuera a nivel colegio) y construir los indicadores de violencia directamente desde el Censo Educativo, a nivel colegio.

---

## Fuentes de datos

| Fuente | Contenido | Nivel original |
|---|---|---|
| **Censo Educativo 2025 — Módulo I** (`Lineal_3AP_1`) | Respuestas del director sobre convivencia escolar, tutoría y ESI | Colegio |
| **Padrón de IIEE** (`Padron_web`) | Datos administrativos y geográficos | Colegio, todos los niveles |
| **Matrícula 2025** (`Matricula_01`, cédula 3AP cuadro C201) | Matrícula por colegio y turno (denominador de las tasas) | Colegio × turno |
| **Resultado del Ejercicio 2025** (cédula 3BP, fuente SIAGIE) | Promoción, repetición, retiro | Colegio × cuadro × situación (formato largo) |

- Origen: [ESCALE – MINEDU](https://escale.minedu.gob.pe/)
- Bases procesadas (`.parquet`, filtradas a Primaria): [Hugging Face – themasterdrop/rompebrechas-violencia-escolar](https://huggingface.co/datasets/themasterdrop/rompebrechas-violencia-escolar)

El notebook lee las cuatro fuentes directamente desde Hugging Face; no requiere descargar los `.dbf` originales.

---

## Estructura del notebook

| Sección | Contenido |
|---|---|
| 0 | Cambios respecto al Hito 1 y preparación del entorno |
| 1 | Carga, limpieza y construcción de variables — Censo Educativo (convivencia y violencia) |
| 2 | Carga y limpieza — Resultado del Ejercicio Educativo (repetición, retiro) |
| 3 | Carga y limpieza — Matrícula (denominador de las tasas) |
| 4 | Carga y limpieza — Padrón de IIEE (ubicación, gestión) |
| 5 | Integración de las cuatro fuentes, validación de cruces y construcción de indicadores |
| 6 | EDA sobre la base integrada (5 gráficos con conclusiones) |
| 7 | Transformación de variables: codificación, discretización, log, escalamiento |
| 8 | Construcción de la matriz analítica y su diccionario |
| 9 | Próximos pasos |
| 10 | Conclusiones |

---

## Cómo ejecutar

### Opción 1 — Google Colab (recomendada)

1. Abrir el notebook directamente desde GitHub en Colab: `Archivo > Abrir notebook > GitHub` y pegar la URL de este repositorio, o usar el enlace `[COMPLETAR: URL de Colab]`.
2. Ejecutar todas las celdas (`Entorno de ejecución > Ejecutar todas`). No requiere instalar nada: Colab ya trae `pandas`, `numpy`, `plotly`, `statsmodels` y `scikit-learn`.
3. Requiere conexión a internet, porque las cuatro fuentes se leen directamente desde Hugging Face.

### Opción 2 — Entorno local

**Requisitos**

```bash
pip install pandas numpy plotly statsmodels scikit-learn pyarrow jupyter
```

**Ejecución**

1. Clonar el repositorio:
   ```bash
   git clone [COMPLETAR: URL del repositorio GitHub]
   cd rompebrechas-hito2
   ```
2. Abrir `Rompebrechas_Hito2.ipynb` en Jupyter (`jupyter notebook` o `jupyter lab`).
3. Ejecutar las celdas en orden. La primera celda de código crea la carpeta `salidas/` y define la conexión al repositorio de Hugging Face; el resto de las fuentes se descargan automáticamente desde ahí (no se necesita ningún archivo local).
4. Requiere conexión a internet para leer los `.parquet` publicados en Hugging Face.

### Salidas generadas

Al ejecutar el notebook completo, se guardan en `salidas/`:

- **`base_integrada.parquet`**: base integrada a nivel colegio (censo + padrón + matrícula + resultados), sin transformar.
- **`matriz_analitica.csv`**: matriz analítica final (37,354 colegios × 31 columnas).
- **`diccionario_matriz.csv`**: diccionario de variables de la matriz analítica.

---

## Metodología destacada

- **Tratamiento de preguntas condicionadas**: `P124B_SI` y `P126B_SI_1` distinguen tres significados del vacío (0 estructural, 0 sin anotar, o no respondió), en vez de imputar la mediana como en el Hito 1.
- **Detección de atípicos con reglas de negocio en vez de IQR** cuando la variable está concentrada en cero (más del 85% de colegios registra cero incidencias).
- **Integración validada** con `validate="one_to_one"` y verificación de duplicados en cada cruce, documentando qué colegios quedan fuera y por qué.
- **Estadísticos robustos a la concentración en cero**: tasa agregada (no promedio de tasas) y correlación de Spearman (no Pearson).
- **Transformaciones aplicadas solo donde funcionan**: log para la matrícula (asimetría de 3.6 a 0.1), discretización por reglas de negocio para las incidencias (el log y los cuantiles no sirven por la masa de ceros), z-score solo sobre variables sin masa de ceros.
- **Regresión logística** para estimar el efecto de cada factor institucional sobre la probabilidad de registrar, controlando por los demás.

---

## Principales hallazgos

1. El registro de violencia escolar depende más de la **capacidad y el tamaño del colegio** que de la violencia misma: el registro va de 5% en colegios muy pequeños a 45% en los grandes, y de 3% a 15% según el índice de gestión de la convivencia.
2. Los factores institucionales asociados al registro (odds ratio, controlando por los demás) son: tamaño (1.6 por cada duplicación de la matrícula), tener responsable de convivencia (1.6), conocer los protocolos (1.4) y hacer actividades de ESI con familias (1.3).
3. Existe una brecha consistente entre el libro de incidencias y el portal SíseVe: en los 25 departamentos se registran más incidencias en el libro (razón de 2.6 a 1), lo que sugiere que el SíseVe subestima el fenómeno.
4. Las tasas más altas no coinciden con los conteos absolutos más altos: Lima es el puesto 1 en incidencias pero el 16 en tasa; las tasas más altas están en Pasco, Loreto, Ucayali y Amazonas.
5. La relación entre violencia registrada y trayectoria escolar (repetición, retiro) es muy débil (Spearman ≈ 0.06–0.09).

## Limitaciones

- Los indicadores de violencia son **autodeclarados por el director** y miden violencia *registrada*, no *ocurrida*.
- 16% de los colegios no tiene dato de incidencias, y la falta de respuesta no es aleatoria (mayor en colegios pequeños, rurales y de la Amazonía).
- El análisis es transversal (incidencias de 2024, matrícula y resultados de 2025); no permite afirmar causalidad.
- La regresión logística no controla por departamento.

## Próximos pasos (no implementados en este hito)

1. Reestimar la regresión logística agregando control por departamento (`C(D_DPTO)`).
2. Segmentar colegios con K-means sobre las variables escaladas (`_z`) y las binarias de gestión de la convivencia, para identificar perfiles de "capacidad de registro".

---

## Estructura del repositorio

```
.
├── Rompebrechas_Hito2.ipynb      # Notebook principal (código fuente documentado)
├── README.md                     # Este archivo
├── docs/
│   └── Rompebrechas_Hito2.docx   # Documento Word: desarrollo y justificación de decisiones
└── salidas/                      # Generada al ejecutar el notebook
    ├── base_integrada.parquet
    ├── matriz_analitica.csv
    └── diccionario_matriz.csv
```
## Autoría

Grupo Rompebrechas — Curso Data Mining, Universidad del Pacífico.
Mateo Pereyra · Raúl Porras · Mauro Martínez.

