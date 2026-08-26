# Rompebrechas — Violencia escolar en el Perú (2024–2025)

Proyecto del curso Data Mining (Universidad del Pacífico). Analiza casos de violencia
escolar reportados a nivel nacional en el período 2024–2025 y su relación con las
características de las instituciones educativas (tipo de gestión, nivel, ubicación
geográfica), como línea base para futuras estrategias de prevención.

## Problema

¿Qué factores influyen en los casos de violencia escolar a nivel nacional en niños de
educación primaria durante el período 2024–2025?

## Fuentes de datos

| Fuente | Institución | Enlace |
|---|---|---|
| Censo Educativo 2025 (Lineal_3AP) | MINEDU — ESCALE | https://escale.minedu.gob.pe/uee/-/document_library_display/GMv7/view/10632985 |
| Denuncias de violencia escolar (SíseVe) | MINEDU — SíseVe | https://siseve.minedu.gob.pe/Web/App/Mapa |
| Padrón de Instituciones Educativas | MINEDU — ESCALE | https://escale.minedu.gob.pe/web/inicio/padron-de-iiee |

El detalle de cada fuente (descripción, variables, forma exacta de acceso) está en el
documento Word de la Entrega 1.

## Estructura del repositorio

```
├── Rompebrechas_Final.ipynb   # Notebook con todo el proceso
├── Rompebrechas_Entrega1_Hito_formativo.docx
├── data/
│   ├── Lineal_3AP_1.dbf / .zip
│   ├── Padron_web.dbf / .zip
│   └── denuncias_escolares.xlsx
└── README.md
```

> Los archivos de datos no se incluyen en el repositorio por su tamaño (Padrón Web
> pesa ~16 MB); se descargan desde los enlaces de la tabla anterior y se colocan en
> `data/` antes de ejecutar el notebook.

## Cómo ejecutar el proyecto

1. Clonar el repositorio:
   ```bash
   git clone <URL_DEL_REPOSITORIO>
   cd rompebrechas
   ```
2. Descargar las 3 fuentes de datos desde los enlaces de la tabla y colocarlas en
   `data/` (o en la misma carpeta que el notebook si se usa Google Colab).
3. Abrir `Rompebrechas_Final.ipynb` en Google Colab o Jupyter.
4. Instalar las dependencias que no vienen preinstaladas:
   ```python
   !pip install dbfread plotly
   ```
5. **Antes de ejecutar todo:** en la celda del segundo gráfico (comparación por
   gestión en Lima Metropolitana) falta definir `data_lima_metro`. Agregar esta
   línea justo antes de esa celda:
   ```python
   data_lima_metro = base_censo_final[base_censo_final["D_REGION"] == "DRE LIMA METROPOLITANA"].copy()
   ```
6. Ejecutar todas las celdas en orden (`Entorno de ejecución → Ejecutar todas` en
   Colab). El notebook cubre, en este orden:
   - carga de las 3 fuentes;
   - inspección inicial (tamaño, tipos, faltantes, valores distintos);
   - detección de problemas de calidad (duplicados, errores de registro, valores
     fuera de rango, categorías no uniformes) por fuente;
   - limpieza y decisiones documentadas por fuente;
   - validación de la clave de integración (`COD_MOD` + `ANEXO`) y de duplicados;
   - integración de las 3 fuentes: censo + padrón por colegio (llave `COD_MOD` +
     `ANEXO`), y denuncias agregadas por **UGEL** (`UGEL_norm`) unidas a la base
     escolar;
   - 3 visualizaciones con Plotly: denuncias por tipo de violencia y departamento;
     promedio de denuncias por tipo de gestión en Lima Metropolitana; y box plot
     de esa misma comparación;
   - hallazgos preliminares.

## Unidad de análisis

- Censo (Lineal_3AP) y Padrón Web: cada fila es un colegio (llave `COD_MOD`+`ANEXO`).
- Denuncias (SíseVe): cada fila es un caso de denuncia (sin identificador de colegio,
  por lo que se integra por **UGEL**, no por colegio).

## Limitaciones conocidas

- Solo el 78.2% de las UGEL de las denuncias coincide textualmente con las UGEL del
  padrón (diferencias de tildes/formato en el nombre); el resto queda sin dato de
  denuncias en la base integrada.
- Al sumar el conteo de denuncias por departamento, el resultado se infla porque cada
  colegio de una misma UGEL repite el mismo valor agregado — los totales absolutos del
  gráfico 1 deben leerse en términos relativos entre departamentos, no como cifras
  reales.
- La comparación de gestión pública vs. privada (gráficos 2 y 3) está confundida por
  este mismo efecto: la mediana de denuncias es igual en las tres categorías de
  gestión, así que la diferencia de promedios refleja cómo se distribuyen los colegios
  entre UGEL, no un patrón real asociado al tipo de gestión.



## Autoría

Grupo Rompebrechas — Curso Data Mining, Universidad del Pacífico.
Mateo Pereyra · Raúl Porras · Mauro Martínez.

