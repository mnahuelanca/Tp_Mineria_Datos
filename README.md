# Trabajo Práctico de Minería de Datos

## Tema

Análisis del rendimiento académico de estudiantes a partir de sus calificaciones, hábitos de estudio, asistencia y factores contextuales.

## Objetivo

Identificar de manera anticipada el nivel de rendimiento académico final de un estudiante, representado por la variable `Calificacion`, con categorías `A`, `B`, `C`, `D` y `F`.

El análisis busca explorar qué variables académicas, conductuales y sociodemográficas pueden estar relacionadas con la calificación final y servir como potenciales predictores.

## Dataset

El dataset utilizado es **Students Grading Dataset**, disponible en Kaggle:

<https://www.kaggle.com/datasets/mahmoudelhemaly/students-grading-dataset>

La base contiene 5.000 observaciones y 23 variables, entre ellas:

- datos de identificación: estudiante, nombre, apellido y correo electrónico;
- características personales y académicas: género, edad, departamento y asistencia;
- calificaciones: nota parcial, nota final, promedio de tareas, promedio de cuestionarios, participación, proyectos y puntaje total;
- hábitos y contexto: horas de estudio semanales, actividades extracurriculares, acceso a Internet, educación de los padres, ingresos familiares, nivel de estrés y horas de sueño;
- variable objetivo: `Calificacion`.

El archivo local utilizado por el notebook es `datos/estudiantes.csv`. Las columnas originales, que están en inglés, se renombran al español durante el análisis para facilitar su interpretación.

## Documentación CRISP-DM

[CRISP-DM.md](CRISP-DM.md) resume el proyecto mediante las seis etapas de la metodología.

## Trabajo realizado

El notebook `tp.ipynb` incluye actualmente:

1. carga del archivo CSV con pandas;
2. traducción y normalización de los nombres de las columnas;
3. inspección de la estructura, tipos de datos y dimensiones;
4. análisis de valores faltantes y valores únicos;
5. estadística descriptiva de las variables numéricas;
6. exploración de la distribución de las calificaciones;
7. visualizaciones con Matplotlib y Seaborn;
8. revisión de rangos e identificadores;
9. comparación de la ausencia de datos según calificación e ingresos;
10. revisión de la coherencia entre puntaje total y calificación;
11. interpretaciones y limitaciones de cada análisis.

La parte 2 agrega selección de predictores, partición train-test, preparación de los datos, un árbol de decisión y K-means. Cada sección contiene código, resultados e interpretación de sus gráficos.

### Valores faltantes

El CSV se carga con `keep_default_na=False` y `na_values=['']` para distinguir campos vacíos de categorías válidas. Según los metadatos, `None` en educación de los padres significa sin educación formal: no es un dato faltante.

| Variable | Faltantes | Porcentaje |
| --- | ---: | ---: |
| Asistencia (%) | 516 | 10,32% |
| Promedio_Tareas | 517 | 10,34% |
| Nivel_Educativo_Padres | 984 | 19,68% |

Hay 2.017 celdas faltantes en total. Los 810 registros de educación de los padres con categoría `None` se conservan por separado. Las tablas de ausencia son exploratorias y no demuestran mecanismos MCAR, MAR o MNAR.

### Hallazgos y limitaciones

No se encontraron filas duplicadas, identificadores o correos repetidos, ni valores fuera de las escalas revisadas. No aparecen outliers con la regla de 1,5 veces el rango intercuartílico. La variable objetivo está completa; A es la clase más frecuente, con 29,90% de los registros.

La asistencia observada es mayor para A y B, aunque esta asociación no demuestra causalidad. El puntaje total presenta distribuciones similares en todas las calificaciones. Los metadatos no especifican cómo se construyen las letras, por lo que no se asume una conversión mediante umbrales ni se reemplaza el target.

## Ejecución

Abrir `tp.ipynb` desde la carpeta del proyecto y ejecutar todas las celdas en orden. El dataset se carga una sola vez. El notebook incluye los resultados y gráficos de la ejecución verificada.

## Parte 2: preparación y aprendizaje

### Variables y limpieza

Se conservan los 5.000 estudiantes. Los modelos utilizan copias del dataset, sin ID, nombre, apellido, correo ni calificaciones académicas. El target es `Calificacion`; los 11 predictores son edad, asistencia, horas de estudio, estrés, sueño, género, departamento, actividades extracurriculares, acceso a Internet, educación de los padres e ingresos familiares.

En educación de los padres, `None` se renombra como `Sin educación formal` y los campos vacíos como `No informado`. La asistencia faltante se completa con la mediana de train (75,71%), utilizando ese mismo valor en test. Promedio de tareas queda excluido y no requiere imputación para estos modelos. El dataset original `df` permanece sin cambios.

Se divide en 80% train (4.000 estudiantes) y 20% test (1.000), con estratificación por calificación y `random_state=42`. La mediana, el codificador de categorías y el escalador se ajustan únicamente con train. No se eligen parámetros a partir de test.

### Aprendizaje supervisado: árbol de decisión

Se utiliza un árbol de profundidad máxima 3 y al menos 50 observaciones por hoja para facilitar la lectura de las reglas. Las categorías se convierten a columnas binarias mediante one-hot encoding aprendido en train; las variables numéricas no requieren estandarización para el árbol.

| Evaluación | Accuracy | F1 macro |
| --- | ---: | ---: |
| Árbol en train | 0,393 | 0,315 |
| Árbol en test | 0,368 | 0,290 |
| Referencia en test: predecir siempre A | 0,299 | 0,092 |

El árbol mejora 6,9 puntos porcentuales sobre la referencia, pero su capacidad predictiva es limitada: no predice la clase C y depende fuertemente de la asistencia. La referencia es una regla simple de comparación, no un segundo modelo entrenado. El notebook incluye reporte por clase, matriz de confusión, reglas e importancia de variables.

### Prueba complementaria sin asistencia

Antes de K-means se agregó un bloque que repite el árbol sin `Asistencia (%)`, conservando la misma partición, preparación y parámetros. La accuracy en test baja de 36,8% a 28,6% y el F1 macro de 0,290 a 0,107. Sin esa variable, el árbol predice A en 919 de los 1.000 casos y B en los 81 restantes, sin identificar C, D o F.

La comparación muestra la dependencia de la asistencia en esta prueba, sin demostrar causalidad. Es exploratoria porque reutiliza el test ya evaluado; no se utiliza para ajustar parámetros ni para reemplazar el modelo principal. Los modelos anteriores y K-means se conservan.

### Aprendizaje no supervisado: K-means

K-means utiliza las cinco variables numéricas (edad, asistencia, estudio, estrés y sueño), estandarizadas con la media y desviación de train. Las variables categóricas sirven para describir los perfiles después y la calificación no participa en su formación.

Se comparan k=2 a k=6 con el mismo algoritmo, diez inicializaciones y semilla 42. Se selecciona k=6 por el mayor silhouette en una muestra fija de 2.000 estudiantes de train: 0,151. Los estudiantes de test se asignan a los centroides aprendidos y obtienen silhouette 0,144.

Los valores son bajos y cercanos entre las alternativas: la separación es débil y no se puede afirmar que existan seis tipos naturales de estudiantes. El notebook muestra sus tamaños, medias, categorías más frecuentes y distribución posterior de calificaciones. Los grupos no equivalen a las clases A/B/C/D/F.

### Límites del objetivo anticipado

La intención es predecir en una instancia intermedia del curso, pero el archivo no especifica cuándo se midieron asistencia, hábitos y contexto. Por eso la evaluación es retrospectiva y no valida una predicción anticipada real. También permanece la limitación de coherencia entre puntaje total y calificación. Los resultados son exploratorios y se basan en una sola partición del dataset.

## Tecnologías utilizadas

- Python
- Jupyter Notebook
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn

Verificado con Python 3.12 y scikit-learn 1.8. El código de `OneHotEncoder` utiliza `sparse_output`, disponible desde scikit-learn 1.2.
