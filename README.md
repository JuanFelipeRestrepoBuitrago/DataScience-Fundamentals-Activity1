# Taller Práctico 01 — Equipo Movilidad Urbana

- **Curso:** Fundamentos en Ciencia de Datos — Maestría en Ciencia de Datos y Analítica, EAFIT
- **Conjunto de datos elegido:** C — Movilidad urbana
- **Fecha límite de entrega:** lunes 27 de julio de 2026, 23:50
- **Fecha de entrega:** 26 de julio de 2026

## Integrantes

| Nombre completo | Cédula |
|---|---:|
| Juan Felipe Restrepo | 1027740136 |
| Manuela Castaño | 1011510403 |
| Juan Esteban García | 1020222158 |

## 1. Resumen ejecutivo

Se analizaron 1.424 lecturas depuradas de seis sensores de movilidad para identificar dónde conviene probar semaforización inteligente. El tráfico alto se definió como superar 34 vehículos por intervalo. La probabilidad observada de superar ese umbral fue 72,59 % en las franjas 06:00–08:00 y 16:00–18:00, frente a 0 % en el resto de horarios del periodo analizado. Av. Regional alcanzó 90 % a las 18:00 y Av. Oriental 85 % a las 08:00. Se recomienda iniciar allí un piloto reversible, con horarios de control y medición de tiempos de viaje, colas y efectos sobre vías transversales.

## 2. Pregunta de negocio

- **Pregunta ancla:** ¿En qué corredores y horarios se debe pilotear semaforización inteligente?
- **Pregunta específica:** ¿En qué corredores y franjas horarias es mayor la probabilidad de observar un conteo superior al tercer cuartil de los datos depurados y, por tanto, dónde conviene iniciar un piloto de semaforización inteligente?
- **Evento de decisión:** tráfico alto = más de 34 vehículos por intervalo.

## 3. Estructura del repositorio

```text
.
├── README.md
├── requirements.txt
├── data/
│   ├── raw/
│   │   ├── movilidad_sensores_LIMPIO.csv
│   │   ├── movilidad_sensores_CONTAMINADO.csv
│   │   └── clima_api_log.json
│   └── processed/
│       └── contaminado_transformado.csv
├── notebooks/
│   └── taller_practico_01_analisis.ipynb
├── results/
│   ├── figuras/
│   └── tabla_diagnostico_gigo.csv
├── taller_practico/
│   └── Taller_Practico_01.tex
└── docs/
    └── declaracion_uso_IA.md
```

Las respuestas del documento de tres partes se encuentran al final del notebook, en la sección `## Taller Práctico 01 — Respuestas`. El archivo `.tex` original se conserva como referencia del enunciado, siguiendo la opción Markdown permitida por la guía.

## 4. Cómo reproducir el análisis

### Terminal

```bash
git clone https://github.com/JuanFelipeRestrepoBuitrago/DataScience-Fundamentals-Activity1.git
cd DataScience-Fundamentals-Activity1

python -m venv .venv
```

Activar el entorno:

```bash
# Linux / macOS
source .venv/bin/activate

# Windows PowerShell
.\.venv\Scripts\Activate.ps1
```

Instalar las dependencias y abrir el notebook:

```bash
python -m pip install -r requirements.txt
jupyter notebook notebooks/taller_practico_01_analisis.ipynb
```

Ejecute todas las celdas en orden. El notebook genera de forma reproducible:

- `data/processed/contaminado_transformado.csv`
- `results/tabla_diagnostico_gigo.csv`
- Las visualizaciones finales en `results/figuras/`

### Google Colab

1. Subir el notebook y crear la ruta `data/raw/`.
2. Cargar en esa ruta los tres archivos originales de `data/raw/`.
3. Ejecutar las celdas en orden mediante **Runtime → Run all**.
4. Descargar los archivos generados desde `data/processed/` y `results/`.

El código contempla tanto la ejecución desde la raíz del proyecto como desde el directorio `notebooks/`.

## 5. Principales hallazgos

| # | Hallazgo | Evidencia |
|---:|---|---|
| 1 | Los conteos más altos se concentran entre 06:00–08:00 y 16:00–18:00; en esas franjas la probabilidad de superar 34 vehículos es 72,59 %. | `results/figuras/trafico-promedio-por-hora.png` y tabla de decisión del notebook |
| 2 | Av. Regional a las 18:00 presenta la mayor probabilidad observada de tráfico alto: 90 % (18 de 20 lecturas). | Tabla “Combinaciones corredor–hora” del punto 5 |
| 3 | Los promedios por sensor son cercanos; la variación temporal es más relevante que la diferencia del promedio global entre corredores. | `results/figuras/mapa-sensores-trafico-promedio.png` |

## 6. Problemas de calidad encontrados

| Problema | Estrategia de corrección | Justificación |
|---|---|---|
| Nulos en `conteo_vehiculos` | Mediana por `sensor_id` | Conserva el comportamiento típico de cada sensor y es robusta frente a extremos. |
| Nulos en `temperatura_c` | Mediana por `sensor_id` | Evita que sensores con distribuciones distintas compartan una imputación global. |
| Nulos en `condicion_clima` | Moda por `sensor_id` | Es una variable nominal y la categoría más frecuente es una imputación trazable. |
| Duplicados exactos | Conservar una copia | No aportan información nueva y duplican el peso del evento. |
| Duplicados de negocio | Validación con `sensor_id` + `timestamp` | Un sensor debe producir una sola lectura por instante. La validación se repite después de normalizar fechas. |
| Categorías climáticas inconsistentes | Diccionario de mapeo | Unifica mayúsculas, minúsculas y sinónimos observados. |
| Fechas heterogéneas | Conversión explícita de formatos | Permite analizar horarios sin descartar automáticamente los 90 registros afectados. |
| Conteos negativos | Eliminación | Son físicamente imposibles y no pueden reconstruirse sin inventar información. |
| Valores `99999` | Eliminación como código centinela | Los cuatro extremos comparten exactamente el mismo valor y son incompatibles con la escala del proceso. |
| Coordenadas invertidas | Intercambio condicionado por rangos | Solo se corrigen cuando cada coordenada encaja inequívocamente en el rango opuesto de Medellín. |

El detalle reproducible se exporta en `results/tabla_diagnostico_gigo.csv`.

## 7. Decisión recomendada

- **Recomendación:** iniciar un piloto controlado en Av. Regional durante 16:00–18:00 y en Av. Oriental durante 06:00–08:00. Comparar con días u horarios de control antes de ampliarlo.
- **Falso Positivo:** invertir y modificar ciclos donde no existe congestión persistente, aumentando esperas en vías transversales o cruces peatonales.
- **Falso Negativo:** no intervenir una franja realmente congestionada, manteniendo demoras, consumo de combustible, emisiones y riesgo de incidentes.
- **Limitación principal:** la muestra cubre seis sensores y veinte días, con lecturas cada dos horas; no contiene tiempos de viaje, longitud de colas, incidentes ni ciclos semafóricos. Las probabilidades describen el periodo observado y no demuestran causalidad.

## 8. Uso de Inteligencia Artificial

Se utilizó ChatGPT (OpenAI) como apoyo para estructurar código de limpieza, normalización de fechas, validaciones, agregaciones, visualizaciones y revisión de redacción. El equipo definió las reglas de negocio, ejecutó el notebook completo y validó las cifras y conclusiones. La declaración ampliada se encuentra en `docs/declaracion_uso_IA.md`.
