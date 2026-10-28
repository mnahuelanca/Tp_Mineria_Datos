# Proyecto de minería de datos con CRISP-DM

**Tema:** análisis del rendimiento académico de estudiantes.

CRISP-DM organiza un proyecto de minería de datos en seis etapas. No es una secuencia rígida: los resultados pueden llevarnos a revisar decisiones anteriores. A continuación se describe cómo se aplicó al trabajo. Referencia: [guía CRISP-DM de IBM](https://www.ibm.com/docs/en/SS3RA7_19.0.0/pdf/ModelerCRISPDM.pdf).

## 1. Comprensión del problema

El objetivo es explorar si la asistencia, los hábitos y el contexto de los estudiantes permiten anticipar su calificación, expresada como A, B, C, D o F. También se busca encontrar perfiles de estudiantes con características similares.

En un contexto educativo, esto podría ayudar a orientar acciones de acompañamiento. Para este trabajo universitario, buscamos aplicar e interpretar un árbol de decisión y K-means, sin asumir que sus resultados sean suficientes para tomar decisiones sobre estudiantes reales. Como referencia mínima, comparamos el árbol con predecir siempre la calificación más frecuente: A.

## 2. Comprensión de los datos

Se utilizó un dataset de Kaggle con 5.000 estudiantes y 23 variables. Se revisaron las columnas, los tipos de datos, los duplicados, los valores faltantes y las distribuciones mediante tablas y gráficos.

No se encontraron registros duplicados. Se detectaron faltantes en asistencia, tareas y educación de los padres. También se aclaró que el texto `None` en educación de los padres significa sin educación formal y no es un dato faltante. La asistencia mostró diferencias entre calificaciones, mientras que el puntaje total no tuvo una correspondencia clara con las letras del resultado final.

## 3. Preparación de los datos

Se seleccionaron 11 características y se excluyeron identificadores, nombres, correos y notas académicas. Se conservaron todos los estudiantes y se trabajó sobre copias del dataset original.

Se separaron 4.000 estudiantes para entrenamiento y 1.000 para prueba, manteniendo proporciones similares de calificaciones. Los faltantes de asistencia se completaron con la mediana de entrenamiento (75,71%). En educación de los padres se distinguió entre `Sin educación formal` y `No informado`. Las categorías se convirtieron a columnas binarias para el árbol. Para K-means se estandarizaron las cinco variables numéricas. Los valores necesarios para estas transformaciones se aprendieron solamente con entrenamiento.

## 4. Modelado

Se aplicó un **árbol de decisión** de poca profundidad para predecir la calificación y facilitar la interpretación de sus reglas. Por sugerencia del profesor, se repitió la prueba sin asistencia, conservando la misma partición y los mismos parámetros.

Como técnica no supervisada se utilizó **K-means** para formar perfiles según edad, asistencia, estudio, estrés y sueño. Se compararon entre dos y seis grupos, sin utilizar la calificación para formarlos. La prueba sin asistencia es una variante del árbol; no incorpora otra técnica supervisada.

## 5. Evaluación

| Prueba | Aciertos en test | F1 macro |
| --- | ---: | ---: |
| Árbol con asistencia | 36,8% | 0,290 |
| Árbol sin asistencia | 28,6% | 0,107 |
| Referencia: siempre A | 29,9% | 0,092 |

Sin asistencia el resultado empeora y el árbol predice casi siempre A. Esto muestra su dependencia de esa variable en la comparación realizada, pero no demuestra causalidad. La comparación es exploratoria porque reutiliza el mismo conjunto de prueba; no se utilizó para ajustar parámetros.

K-means seleccionó seis grupos dentro del rango evaluado, pero la separación fue débil. Ambos análisis tienen limitaciones: desconocemos cómo se construyeron las calificaciones y cuándo se midieron los predictores. Por eso no se validó una predicción anticipada real.

## 6. Presentación y posible aplicación

La entrega consiste en el notebook con código, gráficos e interpretaciones, el README y este documento. No se implementó un sistema en una institución educativa.

Antes de una aplicación real habría que revisar la calidad y el origen de los datos, precisar el momento de predicción y evaluar con nuevas muestras.
