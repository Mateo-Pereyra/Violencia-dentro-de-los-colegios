# Rompebrechas: ¿un cero significa que no hubo violencia escolar?

Proyecto del curso **Data Mining** (Universidad del Pacífico). Grupo Rompebrechas: Mateo Pereyra, Raúl Porras y Mauro Martínez.
Docente: Soledad Espezúa Llerena.

**Estado: entrega final (Hito 4).** Resultados definitivos, conclusiones y recomendaciones para la UGEL.

## Problema

El 71.5 % de los colegios de primaria del Perú tiene libro de incidencias y anotó **cero** casos de violencia en 2024, y otro 16.8 % ni siquiera tiene libro. Ese cero puede significar que no hubo violencia o que el colegio no tiene cómo registrarla. Si se lee como "aquí no pasa nada", los colegios con menos capacidad quedan fuera de la atención justamente porque no registran.

- **Pregunta:** ¿qué características institucionales se asocian a que un colegio de primaria registre casos de violencia escolar, y se relaciona ese registro con la repetición y el retiro de sus estudiantes (2024-2025)?
- **Usuarios:** especialistas de convivencia escolar de las UGEL y DRE, y el equipo del MINEDU a cargo del SíseVe.
- **Beneficiarios:** estudiantes de primaria, en especial los de colegios donde hoy la violencia no se registra.
- **Objetivo:** ayudar a la UGEL a priorizar los colegios donde un cero dice poco.

## Datos

Cada fila es un colegio de primaria (`COD_MOD` + `ANEXO`): **37,354 colegios**. Todas las fuentes son públicas.

| Fuente | Qué aporta | Origen |
|---|---|---|
| Censo Educativo 2025 (cédula 3AP) | Respuestas del director sobre convivencia, tutoría y ESI | [ESCALE](https://escale.minedu.gob.pe/uee/-/document_library_display/GMv7/view/10632985) |
| Resultado del Ejercicio Educativo 2025 | Repetición y retiro | [ESCALE](https://escale.minedu.gob.pe/uee/-/document_library_display/GMv7/view/10632985) |
| Matrícula 2025 | Denominador de las tasas | [ESCALE](https://escale.minedu.gob.pe/uee/-/document_library_display/GMv7/view/10632985) |
| Padrón de Instituciones Educativas | Gestión, área, departamento y coordenadas | [ESCALE: Padrón](https://escale.minedu.gob.pe/web/inicio/padron-de-iiee) |
| Límites departamentales (IGN, 2020) | Mapas | `Código/peru_shapes/` |

**No hay que descargar los datos a mano:** los `.dbf` originales se filtraron a primaria y se publicaron en [Hugging Face](https://huggingface.co/datasets/themasterdrop/rompebrechas-violencia-escolar). El notebook los lee de ahí, por lo que necesita internet.

## Técnicas

| Técnica | Pregunta que responde | Entrada |
|---|---|---|
| **K-means (K = 3)**, técnica principal | ¿Qué tipos de colegio hay según su contexto y sus instrumentos de convivencia? | 9 atributos con z-score (37,260 colegios) |
| **Reglas de asociación (Apriori)** | Si a un colegio le falta un instrumento, ¿qué más le falta? | Cada colegio = canasta de instrumentos que le faltan |
| **DBSCAN espacial** | ¿Dónde se juntan muchos colegios sin libro? | Coordenadas en metros de los 6,269 colegios sin libro |

El registro de incidencias, el libro, la repetición y el retiro **no entran** a K-means: son lo que se quiere explicar y solo se usan para interpretar los grupos. K = 3 se eligió por inercia, silueta, Davies-Bouldin y estabilidad (ARI), y los grupos se compararon con el jerárquico de Ward, con PCA y sin escalar.

## Resultados principales

- **Tres perfiles:** *Urbano grande* (11,460), *Rural con instrumentos* (20,725) y *Pocos instrumentos* (5,075). En este último, el 59 % no tiene libro y registra menos que los colegios rurales del mismo tamaño.
- **Las faltas vienen juntas:** cuando faltan dos instrumentos, entre el 70 % y el 82 % de los colegios tampoco tiene libro (16.8 % en el total). Ser rural, por sí solo, casi no lo anticipa.
- **Registrar no va junto con repetir más**, pero el 24 % de los colegios rurales sin libro tiene repetición alta.
- **Dónde:** Amazonía (43 % de los colegios de Loreto en *Pocos instrumentos*) y sierra norte (la zona DBSCAN más grande, 1,689 colegios sin libro).
- **Para la UGEL:** 1,112 colegios sin ninguno de los tres instrumentos; 949 tampoco tienen libro (630 en zonas, 319 aislados).

## Cómo ejecutar

1. Instalar Python 3.11 o superior.
2. Instalar las dependencias:
   ```bash
   pip install -r requirements.txt
   ```
3. Abrir `Código/Rompebrechas_Final.ipynb` en Jupyter o VS Code y ejecutar todas las celdas en orden. Debe ejecutarse **desde la carpeta `Código/`**, porque ahí busca `peru_shapes/` y crea `salidas/`.

**En Google Colab:** subir el notebook y la carpeta `peru_shapes/` (o `peru_shapes.zip`) al panel *Archivos*. La primera celda de código instala `apyori`; el resto de librerías ya viene en Colab.

La ejecución completa toma pocos minutos (la mayor parte es la descarga de los datos).

## Estructura de archivos

```
├── README.md
├── requirements.txt
├── Código/
│   ├── Rompebrechas_Final.ipynb        # notebook ejecutado: de las fuentes al veredicto
│   ├── peru_shapes/                    # límites de los departamentos para los mapas
│   └── salidas/                        # lo que genera el notebook
│       ├── base_integrada.parquet      #   una fila por colegio, ya limpia
│       ├── matriz_analitica.csv        #   matriz analítica + diccionario_matriz.csv
│       ├── colegios_prioritarios.csv   #   lista de 1,112 colegios para la UGEL
│       ├── mapa_ugel_sin_libro.html    #   mapa interactivo (se abre sin Python)
│       └── fig_*.png                   #   figuras del informe
└── Informe/
    ├── Informe_Final_Rompebrechas.tex  # informe en formato IEEE
    ├── Informe_Final_Rompebrechas.pdf
    └── figuras/
```

## Dónde ver cada parte en el notebook

| Parte | Sección |
|---|---|
| Problema, pregunta y fuentes | Planteamiento |
| Limpieza de cada fuente | 1 a 4 |
| Integración e indicadores | 5 |
| El problema en los datos (EDA) | 6 |
| Matriz analítica | 7 |
| K-means: variables, escalamiento, elección de K y perfiles | 8.1 a 8.4 |
| Comparación con otros métodos | 8.5 |
| Reglas de asociación (Apriori) | 8.6 |
| Mapa de los grupos y DBSCAN espacial | 9.1 y 9.2 |
| Mapa interactivo y lista priorizada | 9.3 |
| Veredicto, recomendaciones y limitaciones | 10 |
