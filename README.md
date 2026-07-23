# Taller Práctico 01 — [Nombre del equipo]

- **Curso:** Fundamentos en Ciencia de Datos — Maestría en Ciencia de Datos y Analítica, EAFIT
- **Conjunto de datos elegido:** C - Movilidad
-**Fecha límite de entrega:** Lunes 27 de julio de 2026 23:50 hrs
- **Fecha de entrega real:** [dd/mm/aaaa]

**Integrantes del equipo:**

| Nombre completo | Cédula   |
| --------------- | -------------- |
| Juan Felipe Restrepo | 1027740136 |
| Manuela Castaño | [N° de cédula] |
| Juan Esteban García | [N° de cédula] |

---

## 1. Resumen ejecutivo (máx. 8 líneas)

> Escriba aquí, en lenguaje para un gerente no técnico, cuál era la pregunta de negocio,
> qué encontraron y cuál es la recomendación final. Esta sección se lee primero: debe
> poder entenderse sin abrir el notebook.

## 2. Pregunta de negocio

- **Pregunta ancla del conjunto de datos:** ¿En qué corredores y horarios se debe pilotear semaforización inteligente?
- **Pregunta específica que su equipo decidió responder:** [reformúlenla en términos
  de probabilidad/decisión, no de "cuánto", siguiendo el ejemplo del Taller de Decisión
  de la Sesión 1]

## 3. Estructura del repositorio

```
.
├── README.md
├── data/
│   ├── raw/                  # datos originales (sin modificar)
│   └── processed/            # datos ya limpios, generados por el notebook
├── notebooks/
│   └── taller_practico_01_analisis.ipynb
├── src/                      # funciones auxiliares (opcional)
├── results/
│   └── figuras/
└── docs/
    └── declaracion_uso_IA.md
```

## 4. Cómo reproducir el análisis (vía terminal)

```bash
# 1. Clonar el repositorio
git clone <url-del-repositorio>
cd <nombre-del-repositorio>

# 2. Crear un entorno virtual
python -m venv .venv

# 3. Activar el entorno

# Linux / macOS
source .venv/bin/activate

# Windows
.venv\Scripts\activate

# 4. Instalar dependencias
pip install -r requirements.txt

# 5. Iniciar Jupyter Notebook
jupyter notebook notebooks/taller_practico_01_analisis.ipynb
```
## 4.1. Cómo reproducir el análisis en Google Colab

1. Abrir el notebook `taller_practico_01_analisis.ipynb` en Google Colab.

2. En los archivos de colab agregar 2 carpetas o directorios: `data/raw`.

3. Cargar los archivos datasets desde el equipo local que están en `data/raw` del repositorio en la carpeta de colab creada `data/raw`:
  - `movilidad_sensores_LIMPIO.csv`
  - `movilidad_sensores_CONTAMINADO.csv`
  - `clima_api_log.json`

4. Ejecutar las celdas del notebook en orden, de principio a fin, utilizando **Runtime → Run all** o ejecutándolas una a una.

5. Al finalizar la limpieza de datos, el notebook generará automáticamente el archivo:

  ```
  contaminado_transformado.csv
  ```

  Este archivo corresponde al conjunto de datos limpio y listo para ser utilizado en las siguientes etapas del análisis.

## 5. Principales hallazgos

| #   | Hallazgo | Evidencia (tabla/figura) |
| --- | -------- | ------------------------ |
| 1   |          |                          |
| 2   |          |                          |
| 3   |          |                          |

## 6. Problemas de calidad de datos encontrados (resumen GIGO)

| Problema | Estrategia de corrección | Justificación |
| -------- | ------------------------ | ------------- |
| Valores nulos en `conteo_vehiculos` | Imputación con la mediana por `sensor_id`. | La mediana es robusta frente a valores atípicos y preserva el comportamiento característico de cada sensor. |
| Valores nulos en `temperatura_c` | Imputación con la mediana por `sensor_id`. | Permite conservar la distribución de la temperatura de cada sensor sin verse afectada por valores extremos. |
| Valores nulos en `condicion_clima` | Imputación con la moda por `sensor_id`. | La condición climática más frecuente por sensor representa la mejor aproximación cuando no existe información adicional. |
| Duplicados exactos | Eliminación de registros idénticos. | Los registros repetidos no aportan información nueva y pueden sesgar los resultados del análisis. |
| Duplicados de negocio (`sensor_id` + `timestamp`) | Eliminación de los registros involucrados. | Al existir diferencias en el conteo de vehículos para el mismo sensor y momento, no fue posible determinar cuál registro era el correcto. |
| Categorías inconsistentes en `condicion_clima` | Estandarización mediante un diccionario de mapeo. | Se unificaron diferencias de mayúsculas, minúsculas y sinónimos en las categorías `Soleado`, `Nublado` y `Lluvia`. |
| Formatos heterogéneos de fecha y hora | Conversión a un único formato `datetime`. | Se normalizaron los diferentes formatos de fecha para facilitar el análisis temporal y garantizar consistencia. |
| Conteos negativos de vehículos | Eliminación de registros. | Un conteo negativo representa un valor físicamente imposible y corresponde a un error de captura o medición. |
| Coordenadas geográficas invertidas | Intercambio de los valores de latitud y longitud. | Se recuperaron registros válidos corrigiendo un error evidente de georreferenciación.  |
| Valores atípicos en `conteo_vehiculos` | Eliminación mediante el criterio del rango intercuartílico (IQR). | Se eliminaron únicamente los valores extremos de la variable de interés para reducir el impacto de posibles errores de medición. |

*El diagnóstico completo, los métodos de detección y las evidencias de cada problema se encuentran documentados en el notebook.*

## 7. Decisión recomendada

- **Recomendación:** [acción concreta y accionable]
- **Costo de un Falso Positivo:** [...]
- **Costo de un Falso Negativo:** [...]
- **Limitación principal de los datos que persiste tras la limpieza:** [...]

## 8. Declaración de uso de Inteligencia Artificial

Ver `docs/declaracion_uso_IA.md`. Resumen: [1-2 líneas, ej. "Se usó IA generativa para
sintaxis de pandas en la Tarea 3; la elección de estrategia de imputación y la
interpretación de resultados fue realizada y validada por el equipo."]
