# 🛌 Proyecto Integrador: Análisis de Patrones de Sueño en Estudiantes 

Este proyecto forma parte del **bootcamp de análisis de datos en Unicorn Academy** 🎓. Aquí exploraremos cómo los patrones de sueño afectan el rendimiento académico, utilizando herramientas como SQL para el análisis y GitHub para la documentación.  

## 🚀 Objetivo  
- **Analizar** patrones de sueño registrados en estudiantes.  
- **Limpieza y transformación** de datos para garantizar su calidad.  
- **Relación** con métricas de rendimiento académico (próximos pasos).  

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


--------------------------------------------------

# Rendimiento de los estudiantes en examenes realacinado con el uso de redes sociales 

## 🗂️ Paso 1: Configuración de la base de datos

1. Creación del esquema y las tablas:
Ejecuta el archivo SQL proporcionado (P_FINAL.sql) en tu entorno de base de datos para generar las tablas necesarias:

```
CREATE TABLE estudiantes (
    id_estudiante INT AUTO_INCREMENT PRIMARY KEY,
    genero VARCHAR(10),
    raza_etnicidad VARCHAR(50),
    id_nivel_educativo INT,
    id_tipo_almuerzo INT,
    id_curso_preparacion INT);

CREATE TABLE niveles_educativos_padres (
    id_nivel_educativo INT AUTO_INCREMENT PRIMARY KEY,
    nivel_educativo VARCHAR(100)
);

CREATE TABLE tipos_almuerzo (
    id_tipo_almuerzo INT AUTO_INCREMENT PRIMARY KEY,
    tipo_almuerzo VARCHAR(50)
);

CREATE TABLE calificaciones (
    id_calificacion INT AUTO_INCREMENT PRIMARY KEY,
    id_estudiante INT,
    math_score INT,
    reading_score INT,
    FOREIGN KEY (id_estudiante) REFERENCES estudiantes(id_estudiante)
);

CREATE TABLE cursos_preparacion (
    id_curso_preparacion INT AUTO_INCREMENT PRIMARY KEY,
    estado_preparacion VARCHAR(50)
); 
```

2. Verifica que las tablas hayan sido creadas correctamente:

   ``` SHOW TABLES; ```

## 📝 Paso 2: Inserción de datos

Rellena las tablas con datos simulados o provenientes del dataset del proyecto:

Inserta datos en las tablas principales:

``` INSERT INTO niveles_educativos_padres (nivel_educativo)
SELECT DISTINCT `parental level of education` FROM performance;

INSERT INTO tipos_almuerzo (tipo_almuerzo)
SELECT DISTINCT lunch FROM performance;

INSERT INTO cursos_preparacion (estado_preparacion)
SELECT DISTINCT `test preparation course` FROM performance;

INSERT INTO estudiantes (genero, raza_etnicidad, id_nivel_educativo, id_tipo_almuerzo, id_curso_preparacion)
SELECT 
    gender,
    `race/ethnicity`,
    (SELECT id_nivel_educativo FROM niveles_educativos_padres WHERE nivel_educativo = `parental level of education`),
    (SELECT id_tipo_almuerzo FROM tipos_almuerzo WHERE tipo_almuerzo = lunch),
    (SELECT id_curso_preparacion FROM cursos_preparacion WHERE estado_preparacion = `test preparation course`)
FROM performance;

INSERT INTO calificaciones (id_estudiante, math_score, reading_score)
SELECT 
    id_estudiante,
    `math score`,
    `reading score`
FROM estudiantes
JOIN performance ON estudiantes.genero = performance.gender
AND estudiantes.raza_etnicidad = performance.`race/ethnicity`; '''




