# Proyecto-integrador-
Proyecto final de análisis de datos sobre patrones de sueño en estudiantes y sobre el impacto de la redes sociales en la performance de los estudiantes en exámenes 

# Student Sleep Analysis

Este repositorio contiene el análisis exploratorio y limpieza de datos de patrones de sueño en estudiantes, realizado como parte de un proyecto de análisis de datos.

## Base de datos
- **Nombre del esquema**: `sleeping_patterns`
- **Nombre de la tabla**: `student_sleep_patterns`

## Consultas iniciales

## Paso 1: Ver la estructura de la tabla
### Código utilizando el comando: DESCRIBE 
```sql
DESCRIBE sleeping_patterns.student_sleep_patterns;
```
### Explicación
Este comando permite inspeccionar la estructura de la tabla student_sleep_patterns en el esquema sleeping_pattern. Muestra:

- Nombres de las columnas.
- Tipos de datos.
- Información sobre si permiten valores nulos.

## Paso 2: Obtener una muestra de datos
### Código limitando las primeras 10 filas de la tabla
```SELECT * 
FROM sleeping_patterns.student_sleep_patterns
LIMIT 10;
```
### Explicación:

Selecciona las primeras 10 filas de la tabla para inspeccionar cómo se ven los datos y detectar posibles problemas.

## Paso 3: Estadísticas básicas por columna
### Código

```SELECT 
    COUNT(*) AS total_rows,
    COUNT(DISTINCT student_id) AS unique_students,
    AVG(sleep_duration) AS avg_sleep,
    MIN(sleep_duration) AS min_sleep,
    MAX(sleep_duration) AS max_sleep
FROM sleeping_patterns.student_sleep_patterns;
```

### Explicación:

- COUNT(*): Cuenta el total de filas de la tabla.
- COUNT(DISTINCT student_id): Verifica si hay duplicados en el identificador de estudiantes.
- AVG, MIN, MAX: Calcula el promedio, mínimo y máximo de la duración del sueño registrada.

