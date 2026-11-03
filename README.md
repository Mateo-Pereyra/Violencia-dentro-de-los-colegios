# Rompebrechas: registro de violencia escolar en colegios de primaria del Perú

Proyecto del curso **Data Mining** (Universidad del Pacífico). Grupo Rompebrechas: Mateo Pereyra, Raúl Porras y Mauro Martínez.
Docente: Soledad Espezúa Llerena.

**Estado: Hito 3, revisión metodológica e implementación inicial.** Los resultados son preliminares; las conclusiones se cierran en el Hito 4.

## Problema

La mayoría de colegios de primaria reporta cero casos de violencia escolar. Ese cero puede significar que no hubo violencia o que el colegio no tiene cómo registrarla. Si se lee como "aquí no pasa nada", los colegios con menos capacidad quedan fuera de la atención.

- **Pregunta:** ¿qué características institucionales se asocian a que un colegio registre casos de violencia, y se relaciona ese registro con la repetición y el retiro de sus estudiantes (2024-2025)?
- **Usuarios:** especialistas de convivencia de las UGEL y DRE, y el equipo del MINEDU a cargo del SíseVe.
- **Beneficiarios:** estudiantes de primaria.
- **Objetivo:** agrupar a los colegios según su contexto y sus instrumentos de convivencia, para priorizar dónde un cero es menos confiable.

## Datos

Cada fila es un colegio de primaria (`COD_MOD` + `ANEXO`). Todas las fuentes son públicas y del MINEDU (ESCALE).

| Fuente | Origen |
|---|---|
| Censo Educativo 2025 (cédula 3AP), Matrícula 2025 y Resultado del Ejercicio 2025 | [ESCALE: Censo Educativo](https://escale.minedu.gob.pe/uee/-/document_library_display/GMv7/view/10632985) |
| Padrón de Instituciones Educativas | [ESCALE: Padrón](https://escale.minedu.gob.pe/web/inicio/padron-de-iiee) |
| Límites departamentales (IGN, 2020) | `Código/peru_shapes/` |

**No hay que descargar los datos a mano:** el notebook los lee de [Hugging Face](https://huggingface.co/datasets/themasterdrop/rompebrechas-violencia-escolar), por lo que necesita internet.

## Técnica seleccionada

**K-means** sobre 9 atributos estandarizados con z-score (37,260 colegios): matrícula en logaritmo, área urbana, gestión privada, responsable de convivencia, afiliación al SíseVe, conocimiento de protocolos, reporte a la UGEL, consulta del RNSSC y actividades de ESI con familias. El registro de incidencias y el libro no entran al modelo; solo sirven para interpretar los grupos.

Se comparó K = 2 a 6 (inercia, silueta, Davies-Bouldin y estabilidad), otras dos representaciones, DBSCAN y clustering jerárquico. Además, DBSCAN espacial ubica zonas con muchos colegios sin libro.

## Cómo ejecutar

1. Instalar Python 3.11 o superior.
2. Instalar las dependencias:
   ```bash
   pip install -r requirements.txt
   ```
3. Abrir `Código/Rompebrechas_Hito3.ipynb` en Jupyter o VS Code y ejecutar todas las celdas en orden. Debe ejecutarse **desde la carpeta `Código/`**, porque ahí busca `peru_shapes/` y crea `salidas/`.

En Google Colab, además del notebook, hay que subir la carpeta `peru_shapes/`.

La ejecución completa toma entre 10 y 15 minutos.

## Estructura de archivos

```
├── README.md
├── requirements.txt
├── Código/
│   ├── Rompebrechas_Hito3.ipynb     # notebook ejecutado: de las fuentes a los resultados
│   ├── peru_shapes/                 # límites de los departamentos para los mapas
│   └── salidas/                     # matriz, diccionario y figuras (al ejecutar se generan también otros archivos)
└── Informe/
    ├── Informe_Hito3_Rompebrechas.tex
    ├── Informe_Hito3_Rompebrechas.pdf
    └── figuras/
```

## Dónde ver cada parte en el notebook

| Parte | Sección |
|---|---|
| Limpieza, integración y matriz analítica | 1 a 8 |
| Representación: atributos y exclusiones | 8.2, 9.1, 10.1 a 10.3 y 10.11 |
| Técnica y justificación | 10 (inicio) y 12.4 |
| Configuraciones y métricas | 10.4 a 10.6, 10.9 a 10.12 y 12.2 |
| Resultados preliminares | 10.8, 10.13, 11 y 12 |
| Problemas y trabajo pendiente | 13 |
