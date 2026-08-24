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
├── Rompebrechas_Final_corregido.ipynb   # Notebook con todo el proceso
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
3. Abrir `Rompebrechas_Final_corregido.ipynb` en Google Colab o Jupyter.
4. Instalar las dependencias que no vienen preinstaladas:
   ```python
   !pip install dbfread plotly
   ```
5. Ejecutar todas las celdas en orden (`Entorno de ejecución → Ejecutar todas` en
   Colab). El notebook cubre, en este orden:
   - carga de las 3 fuentes;
   - inspección inicial (tamaño, tipos, faltantes, valores distintos);
   - detección de problemas de calidad (duplicados, errores de registro, valores
     fuera de rango, categorías no uniformes) por fuente;
   - limpieza y decisiones documentadas por fuente;
   - validación de la clave de integración (`COD_MOD` + `ANEXO`) y de duplicados;
   - integración de las 3 fuentes (censo + padrón por colegio, denuncias agregadas
     por DRE);
   - 3 visualizaciones iniciales con Plotly;
   - hallazgos preliminares.
6. La base integrada final se exporta como `base_integrada_rompebrechas.csv`.

## Unidad de análisis

- Censo (Lineal_3AP) y Padrón Web: cada fila es un colegio (llave `COD_MOD`+`ANEXO`).
- Denuncias (SíseVe): cada fila es un caso de denuncia (sin identificador de colegio,
  por lo que se integra a nivel de DRE).

## Próximos pasos

- Explorar acceso a una fuente de denuncias con mayor detalle geográfico (UGEL o
  código modular) para bajar el análisis a nivel de colegio.
- Construir tasas de denuncia por colegio (no solo conteos absolutos) para comparar
  regiones de forma más justa.
- Avanzar hacia un análisis de segmentación o detección de outliers regionales.

## Autoría

Grupo Rompebrechas — Curso Data Mining, Universidad del Pacífico.
Docente: Soledad Espezúa Llerena.
