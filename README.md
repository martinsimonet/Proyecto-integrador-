# 🛌 Proyecto Integrador: Análisis de Patrones de Sueño en Estudiantes 

Este proyecto forma parte del **bootcamp de análisis de datos en Unicorn Academy** 🎓. Aquí exploraremos cómo los patrones de sueño afectan el rendimiento académico, utilizando herramientas como SQL para el análisis y GitHub para la documentación.  

## 🚀 Objetivo  
- **Analizar** patrones de sueño registrados en estudiantes.  
- **Limpieza y transformación** de datos para garantizar su calidad.  
- **Relación** con métricas de rendimiento académico (próximos pasos).  
---

## Índice del Proyecto
1. [🔑 Seleccionar esquema](sql/1-seleccionar-esquema)  
2. [🏗️ Creación de tablas](sql/2-creación-de-tablas)  
3. [📥 Popular tablas](sql/3-popular-tablas)  
4. [✅ Verificación de tablas](sql/4-verificación-tablas)  
5. [🧹 Limpieza de datos](sql/5-limpieza-de-datos)  
---

## 📝 Guía paso a paso  

### 🔨 **Paso 1: Configuración de la base de datos** 
Incluye los pasos iniciales para establecer la base de datos y asegurarte de trabajar en el esquema correcto.

Para asegurarnos de trabajar en el esquema correcto donde almacenaremos nuestras tablas. 

**Código SQL:**

-- Seleccionar el esquema correcto
 ``` USE sleeping_patterns; ```

### Explicación:
Esto asegura que cualquier tabla que creemos o modifiquemos se haga en la base de datos sleeping_patterns.


---



### 🛠️ Paso 2: Creación de tablas
Aquí explicamos cómo se separaron los datos en tres tablas: `students`, `sleep_patterns` y `lifestyle`.
Para mejorar la organización y el análisis de los datos, dividimos la tabla original `student_sleep_patterns` en tres tablas relacionadas:

1. **`students`**: Contiene información básica de los estudiantes.
2. **`sleep_patterns`**: Incluye datos relacionados con los hábitos de sueño.
3. **`lifestyle`**: Registra los hábitos y actividades de los estudiantes.

**Código SQL:**


-- Crear la tabla `students`
```
CREATE TABLE sleeping_patterns.students (
    Student_ID INT PRIMARY KEY,
    Age INT,
    Gender VARCHAR(10),
    University_Year VARCHAR(20)
);
```

-- Crear la tabla `sleep_patterns`
```
CREATE TABLE sleeping_patterns.sleep_patterns (
    Pattern_ID INT AUTO_INCREMENT PRIMARY KEY,
    Student_ID INT,
    Sleep_Duration FLOAT,
    Sleep_Quality INT,
    Weekday_Sleep_Start TIME,
    Weekday_Sleep_End TIME,
    Weekend_Sleep_Start TIME,
    Weekend_Sleep_End TIME,
    FOREIGN KEY (Student_ID) REFERENCES sleeping_patterns.students(Student_ID)
);
```

-- Crear la tabla `lifestyle`
```
CREATE TABLE sleeping_patterns.lifestyle (
    Lifestyle_ID INT AUTO_INCREMENT PRIMARY KEY,
    Student_ID INT,
    Study_Hours FLOAT,
    Screen_Time FLOAT,
    Caffeine_Intake FLOAT,
    Physical_Activity FLOAT,
    FOREIGN KEY (Student_ID) REFERENCES sleeping_patterns.students(Student_ID)
);

```
### ✍️ Explicación:

- **Normalización de datos:** Dividimos la tabla principal en entidades separadas para evitar redundancias.
- **Claves primarias y foráneas:** Establecimos relaciones entre las tablas mediante claves primarias `(Student_ID)` y foráneas `(Student_ID en las tablas relacionadas)`.
- **Relaciones:** Ahora es posible combinar estas tablas mediante consultas `JOIN`.


---

### **📤 Paso 3: Poblar las tablas**
Explicamos cómo transferimos los datos de la tabla original `student_sleep_patterns` a las nuevas tablas.
Transferimos los datos de la tabla original a las nuevas tablas utilizando consultas `INSERT INTO ... SELECT`.

**Código SQL:**

-- Popular la tabla `students`
```
INSERT INTO sleeping_patterns.students (Student_ID, Age, Gender, University_Year)
SELECT DISTINCT Student_ID, Age, Gender, University_Year
FROM sleeping_patterns.student_sleep_patterns;
```

-- Popular la tabla `sleep_patterns`
```
INSERT INTO sleeping_patterns.sleep_patterns (Student_ID, Sleep_Duration, Sleep_Quality, Weekday_Sleep_Start, Weekday_Sleep_End, Weekend_Sleep_Start, Weekend_Sleep_End)
SELECT Student_ID, Sleep_Duration, Sleep_Quality, Weekday_Sleep_Start, Weekday_Sleep_End, Weekend_Sleep_Start, Weekend_Sleep_End
FROM sleeping_patterns.student_sleep_patterns;
```

-- Popular la tabla `lifestyle`
```
INSERT INTO sleeping_patterns.lifestyle (Student_ID, Study_Hours, Screen_Time, Caffeine_Intake, Physical_Activity)
SELECT Student_ID, Study_Hours, Screen_Time, Caffeine_Intake, Physical_Activity
FROM sleeping_patterns.student_sleep_patterns;
```

### ✍️ Explicación:

- **Selección de datos:** Usamos `DISTINCT` para evitar duplicados en la tabla `students`
- **División lógica:** Los datos se asignan a la tabla correspondiente según su propósito.
- **Validación:** Cada tabla ahora contiene únicamente los datos relacionados con su entidad.

---

### 📊 Paso 4: Verificar la estructura y datos
Aquí mostramos cómo verificar que las tablas se hayan creado correctamente y estén bien pobladas.
Después de crear y poblar las tablas, verificamos que todo esté correcto.

**Código SQL:**
-- Revisar las tablas creadas en el esquema `sleeping_pattern`
```
SHOW TABLES FROM sleeping_patterns;
```

-- Revisar las primeras filas de cada tabla
```
SELECT * FROM sleeping_patterns.students LIMIT 10; 
SELECT * FROM sleeping_patterns.sleep_patterns LIMIT 10;
SELECT * FROM sleeping_patterns.lifestyle LIMIT 10;
```

### ✍️ Explicación:

- **`SHOW TABLES`**: Muestra todas las tablas del esquema.
- **`SELECT * ... LIMIT`**: Permite visualizar una muestra de los datos en cada tabla.

---

### 🧹 Paso 5: Limpieza de datos
La limpieza de datos es un paso esencial en cualquier proyecto de análisis. Aquí identificaremos y resolveremos problemas comunes como duplicados, valores nulos y posibles inconsistencias en las tablas.

#### 🔍 Identificación de duplicados
Primero verificamos si hay registros duplicados en cada tabla.

- **Verificar duplicados en la tabla `students`**
```
SELECT Student_ID, COUNT(*)
FROM sleeping_patterns.students
GROUP BY Student_ID
HAVING COUNT(*) > 1;
```

- **Verificar duplicados en la tabla `sleep_patterns`**
```
SELECT Student_ID, COUNT(*)
FROM sleeping_patterns.sleep_patterns
GROUP BY Student_ID
HAVING COUNT(*) > 1;
```

- **Verificar duplicados en la tabla `lifestyle`**
```
SELECT Student_ID, COUNT(*)
FROM sleeping_patterns.lifestyle
GROUP BY Student_ID
HAVING COUNT(*) > 1;
```

- **Eliminar duplicados en la tabla `students`(ejemplo)**
En nuestro caso no hemos encontrado valores duplicados. A modo informativo, para eliminar duplicados se puede realizar de la siguiente manera:
```
DELETE FROM sleeping_pattern.students
WHERE Student_ID NOT IN (
    SELECT DISTINCT Student_ID FROM (
        SELECT Student_ID FROM sleeping_pattern.students
    ) subquery
);
```

### ✍️ Explicación:

- COUNT(*): Cuenta las filas por Student_ID.
- HAVING COUNT(*) > 1: Filtra estudiantes con más de un registro, lo que indica duplicados.

#### 🔎 Detección de valores nulos

- **Verificar valores nulos en la tabla `students`**
```
SELECT * 
FROM sleeping_patterns.students
WHERE Age IS NULL OR Gender IS NULL OR University_Year IS NULL;
```

- **Verificar valores nulos en la tabla `sleep_patterns`**
```
SELECT * 
FROM sleeping_patterns.sleep_patterns
WHERE Sleep_Duration IS NULL OR Sleep_Quality IS NULL
   OR Weekday_Sleep_Start IS NULL OR Weekday_Sleep_End IS NULL; 
```   
- **Verificar valores nulos en la tabla `lifestyle`**
```
SELECT * 
FROM sleeping_patterns.lifestyle
WHERE Study_Hours IS NULL OR Screen_Time IS NULL 
   OR Caffeine_Intake IS NULL OR Physical_Activity IS NULL; 
```
- **En caso de encontrar dunulos plicados, la forma para eliminarlos sería la siguiente: Eliminar nulos en la tabla `students` (ejemplo)** 
```
UPDATE sleeping_patterns.students
SET Age = (SELECT AVG(Age) FROM sleeping_pattern.students)
WHERE Age IS NULL;
```

### ✍️ Explicación:

- IS NULL: Identifica registros con valores faltantes en las columnas clave de cada tabla.
- Revisión por tabla: Analizamos cada tabla individualmente para detectar problemas específicos.
- Reemplazo con promedios: Para columnas numéricas como Age y Sleep_Duration, utilizamos el promedio.
- Reemplazo con valores predeterminados: Para columnas como Study_Hours, asignamos un valor predeterminado, como 0, si faltan datos.

### 🧪 Consistencia de datos 

Por último, verificamos si los datos son consistentes entre las tablas, por ejemplo, si todos los Student_ID de las tablas relacionadas existen en la tabla principal students.

- **Verificar consistencia de Student_ID entre tablas**
```
SELECT sp.Student_ID
FROM sleeping_patterns.sleep_patterns sp
LEFT JOIN sleeping_patterns.students s ON sp.Student_ID = s.Student_ID
WHERE s.Student_ID IS NULL;
```

- **Lógica similar para la tabla `lifestyle`**
```
SELECT ls.Student_ID
FROM sleeping_patterns.lifestyle ls
LEFT JOIN sleeping_patterns.students s ON ls.Student_ID = s.Student_ID
WHERE s.Student_ID IS NULL;
```
### ✍️ Explicación:

- LEFT JOIN: Conectamos las tablas relacionadas con la tabla principal.
- WHERE ... IS NULL: Detecta IDs que no tienen correspondencia, lo que podría indicar errores en las relaciones.

### ✨ Resultado final de la limpieza de datos
Con estos pasos de limpieza, hemos:

- Identificado y eliminado duplicados.
- Detectado y resuelto valores nulos.
- Asegurado consistencia en las relaciones entre tablas.
- Este proceso asegura que los datos estén limpios, bien estructurados y listos para análisis y visualizaciones en Power BI. 🚀

---
