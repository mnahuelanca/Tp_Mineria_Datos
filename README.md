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

## Trabajo realizado

El notebook `tp.ipynb` incluye actualmente:

1. carga del archivo CSV con pandas;
2. traducción y normalización de los nombres de las columnas;
3. inspección de la estructura, tipos de datos y dimensiones;
4. análisis de valores faltantes y valores únicos;
5. estadística descriptiva de las variables numéricas;
6. exploración de la distribución de las calificaciones;
7. visualizaciones con Matplotlib y Seaborn;
8. preparación del análisis de dependencia entre variables y la calificación mediante información mutua.

Durante la inspección inicial se detectaron valores faltantes principalmente en `Asistencia (%)`, `Promedio_Tareas` y `Nivel_Educativo_Padres`. Estos valores deben considerarse en las etapas posteriores de limpieza y modelado.

## Tecnologías utilizadas

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
