## Declaración de uso de Inteligencia Artificial

Durante el desarrollo del taller se utilizó ChatGPT (OpenAI) como herramienta de apoyo técnico y de revisión. Su uso comprendió:

- Estructuración y optimización de fragmentos de Python y pandas.
- Aplanamiento e integración de la fuente JSON.
- Construcción de validaciones para nulos, duplicados, categorías, fechas, valores imposibles y coordenadas.
- Apoyo en la elaboración de agregaciones, tablas de contingencia y visualizaciones.
- Revisión de consistencia entre resultados numéricos, interpretaciones y redacción.
- Estructuración del cálculo de probabilidades empíricas y de los costos de Falso Positivo y Falso Negativo.

El equipo definió las reglas de negocio y las decisiones metodológicas: métodos de imputación, llave de negocio, rangos geográficos, tratamiento de conflictos, definición del evento de tráfico alto y alcance de la recomendación. La IA no decidió qué corredor intervenir ni sustituyó la interpretación del equipo.

### Validación realizada

El notebook se ejecutó de inicio a fin utilizando el entorno virtual del proyecto. Se verificó que:

- El dataset procesado no contenga nulos, conteos negativos, coordenadas fuera del rango definido ni duplicados de negocio.
- Las cifras escritas en Markdown correspondan con las tablas calculadas.
- Las tres visualizaciones del análisis descriptivo sean legibles y tengan una conclusión asociada.
- La recomendación se derive de la evidencia por corredor y horario y declare sus limitaciones.

La responsabilidad sobre el criterio, la interpretación, la decisión final y la defensa oral de los resultados corresponde a los integrantes del equipo.
