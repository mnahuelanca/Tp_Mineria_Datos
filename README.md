# Proyecto de Minería de Datos - Lesiones en Fútbol

## Objetivo del proyecto

Este proyecto busca analizar un conjunto de datos de lesiones de futbolistas para comprender mejor:

- cómo se distribuyen las edades de los jugadores,
- cuáles son las lesiones más frecuentes,
- si existe relación entre la edad del jugador y la duración de la recuperación,
- y qué variables pueden ser útiles para predecir la cantidad de días o partidos perdidos por lesión.

La idea principal es hacer un trabajo de exploración y análisis de datos (EDA) con enfoque en minería de datos aplicada al fútbol.

---

## Dataset

El proyecto trabaja con el archivo:

- `futbol.csv`

Este dataset contiene información como:

- nombre del jugador,
- edad,
- club,
- liga,
- tipo de lesión,
- días de recuperación,
- partidos perdidos,
- temporada,
- entre otros campos relevantes.

---

## Hipótesis de trabajo

Una de las hipótesis que se está evaluando es:

> A mayor edad del futbolista, mayor podría ser la cantidad de días necesarios para recuperarse de una lesión.

Esta hipótesis se está revisando con gráficos y análisis descriptivos para ver si realmente se cumple o si la relación es más débil o variable de lo que parece.

---

## ¿Qué se está haciendo hasta el momento?

### 1. Carga y limpieza inicial del dataset

Se realizó la carga del archivo CSV con pandas y se verificaron aspectos básicos del dataset, como:

- cantidad de filas y columnas,
- tipos de datos,
- valores nulos,
- registros duplicados,
- cantidad de valores únicos por columna.

Esto permite tener una primera idea de la calidad y estructura de los datos antes de analizar tendencias.

### 2. Exploración descriptiva

Se comenzaron a estudiar varios aspectos del dataset, entre ellos:

- distribución de edades de los futbolistas,
- top 10 lesiones más frecuentes,
- comparación entre edad y días de lesión,
- análisis de casos extremos con recuperaciones muy largas.

### 3. Visualización de resultados

Se generaron gráficos para representar:

- distribución de la edad de los jugadores,
- lesiones más comunes,
- relación entre edad y días de recuperación,
- promedio de días de lesión según la edad.

Esto ayuda a interpretar los datos de manera visual y detectar patrones o anomalías.

### 4. Análisis de casos extremos

También se observaron ejemplos de lesiones cuya recuperación superó los 800 días, mostrando casos graves y complejos, en particular relacionados con lesiones de rodilla.

### 5. Definición de próximos pasos

El notebook deja planteados los siguientes pasos futuros:

- verificar el formato de fechas,
- analizar días de recuperación por tipo de lesión,
- estudiar lesiones por posición,
- comparar resultados por liga,
- detectar outliers,
- y definir posibles variables objetivo para modelos predictivos.

Las variables que más interés tienen para predecir son:

- `Games missed` → cuántos partidos pierde un jugador por lesión,
- `Days` → cuántos días dura la recuperación.

---

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn

---

## Estructura del repositorio

- `futbol.csv`: dataset principal
- `Tp.ipynb`: cuaderno de trabajo con el análisis y visualizaciones
- `README.md`: documentación del proyecto

---