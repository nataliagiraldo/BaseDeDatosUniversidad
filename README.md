# BaseDeDatosUniversidad
![DiagramaUniversidadBdd](https://github.com/nataliagiraldo/BaseDeDatosUniversidad/assets/131258170/3455fd9f-95ac-4c81-a2a1-a2d239fe6106)

DDL
DROP DATABASE IF EXISTS universidad;
CREATE DATABASE universidad;
USE universidad;
 
CREATE TABLE IF NOT EXISTS departamento (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);

CREATE TABLE IF NOT EXISTS ciudad (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(25) NOT NULL
);

CREATE TABLE IF NOT EXISTS direccionAlumno (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(25) NOT NULL,
  id_ciudad INT UNSIGNED NOT NULL,
  CONSTRAINT FK_id_ciudad_alumno FOREIGN KEY(id_ciudad) REFERENCES ciudad(id)
);

CREATE TABLE IF NOT EXISTS direccionProfesor (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(25) NOT NULL,
  id_ciudad INT UNSIGNED NOT NULL,
  CONSTRAINT FK_id_ciudad_profesor FOREIGN KEY(id_ciudad) REFERENCES ciudad(id)
);

CREATE TABLE IF NOT EXISTS alumno (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nif VARCHAR(9) UNIQUE,
    nombre VARCHAR(25) NOT NULL,
    apellido1 VARCHAR(50) NOT NULL,
    apellido2 VARCHAR(50),
    fecha_nacimiento DATE NOT NULL,
    sexo ENUM('H', 'M') NOT NULL,
    id_direccion INT UNSIGNED,
    FOREIGN KEY (id_direccion) REFERENCES direccionAlumno(id)
);

CREATE TABLE IF NOT EXISTS profesor (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nif VARCHAR(9) UNIQUE,
    nombre VARCHAR(25) NOT NULL,
    apellido1 VARCHAR(50) NOT NULL,
    apellido2 VARCHAR(50),
    fecha_nacimiento DATE NOT NULL,
    sexo ENUM('H', 'M') NOT NULL,
    id_departamento INT UNSIGNED NOT NULL,
    id_direccion INT UNSIGNED,
    FOREIGN KEY (id_direccion) REFERENCES direccionProfesor(id),
    FOREIGN KEY (id_departamento) REFERENCES departamento(id)
);


CREATE TABLE IF NOT EXISTS telefonoAlumno (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    numero VARCHAR(9) NOT NULL,
    tipo ENUM('casa', 'celular', 'trabajo') NOT NULL,
    id_alumno INT UNSIGNED NOT NULL,
    FOREIGN KEY (id_alumno) REFERENCES alumno(id)
);

CREATE TABLE IF NOT EXISTS telefonoProfesor (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    numero VARCHAR(9) NOT NULL,
    tipo ENUM('casa', 'celular', 'trabajo') NOT NULL,
    id_profesor INT UNSIGNED NOT NULL,
    FOREIGN KEY (id_profesor) REFERENCES profesor(id)
);

 
 CREATE TABLE IF NOT EXISTS grado (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);
 
CREATE TABLE IF NOT EXISTS asignatura (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    creditos FLOAT UNSIGNED NOT NULL,
    tipo ENUM('básica', 'obligatoria', 'optativa') NOT NULL,
    curso TINYINT UNSIGNED NOT NULL,
    cuatrimestre TINYINT UNSIGNED NOT NULL,
    id_profesor INT UNSIGNED,
    id_grado INT UNSIGNED NOT NULL,
    FOREIGN KEY(id_profesor) REFERENCES profesor(id),
    FOREIGN KEY(id_grado) REFERENCES grado(id)
);
 
CREATE TABLE IF NOT EXISTS curso_escolar (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    anyo_inicio YEAR NOT NULL,
    anyo_fin YEAR NOT NULL
);

CREATE TABLE IF NOT EXISTS alumno_se_matricula_asignatura (
    id_alumno INT UNSIGNED NOT NULL,
    id_asignatura INT UNSIGNED NOT NULL,
    id_curso_escolar INT UNSIGNED NOT NULL,
    PRIMARY KEY (id_alumno, id_asignatura, id_curso_escolar),
    FOREIGN KEY (id_alumno) REFERENCES alumno(id),
    FOREIGN KEY (id_asignatura) REFERENCES asignatura(id),
    FOREIGN KEY (id_curso_escolar) REFERENCES curso_escolar(id)
);

DDL

-- Insertar ciudades
INSERT INTO ciudad (nombre) VALUES
('Madrid'),
('Barcelona'),
('Sevilla'),
('Valencia');

-- Insertar departamentos
INSERT INTO departamento (nombre) VALUES
('Informática'),
('Matemáticas'),
('Biología'),
('Química');

-- Insertar direcciones de alumnos
INSERT INTO direccionAlumno (nombre, id_ciudad) VALUES
('Calle A', 1),
('Calle B', 2),
('Calle C', 3),
('Calle D', 4);

-- Insertar direcciones de profesores
INSERT INTO direccionProfesor (nombre, id_ciudad) VALUES
('Av. X', 1),
('Av. Y', 2),
('Av. Z', 3),
('Av. W', 4);

-- Insertar alumnos
INSERT INTO alumno (nif, nombre, apellido1, apellido2, fecha_nacimiento, sexo, id_direccion) VALUES
('12345678A', 'Juan', 'Pérez', 'Gómez', '1990-05-15', 'H', 1),
('87654321B', 'María', 'López', 'Fernández', '1992-08-20', 'M', 2),
('56789123C', 'Ana', 'Martínez', 'García', '1991-03-10', 'M', 3),
('23456789D', 'Pedro', 'Sánchez', 'Rodríguez', '1993-11-05', 'H', 4);

-- Insertar profesores
INSERT INTO profesor (nif, nombre, apellido1, apellido2, fecha_nacimiento, sexo, id_departamento, id_direccion) VALUES
('98765432E', 'Carlos', 'González', 'Ruiz', '1980-02-25', 'H', 1, 1),
('45678912F', 'Laura', 'Díaz', 'Sanz', '1985-09-12', 'M', 2, 2),
('34567891G', 'Pablo', 'Martín', 'López', '1975-12-30', 'H', 3, 3),
('78912345H', 'Sara', 'Fernández', 'Gómez', '1988-06-18', 'M', 4, 4);

-- Insertar teléfonos de alumnos
INSERT INTO telefonoAlumno (numero, tipo, id_alumno) VALUES
('123456789', 'casa', 1),
('987654321', 'celular', 2),
('654789321', 'trabajo', 3),
('321654987', 'casa', 4);

-- Insertar teléfonos de profesores
INSERT INTO telefonoProfesor (numero, tipo, id_profesor) VALUES
('111222333', 'casa', 1),
('444555666', 'celular', 2),
('777888999', 'trabajo', 3),
('000999888', 'casa', 4);

-- Insertar grados
INSERT INTO grado (nombre) VALUES
('Ingeniería Informática'),
('Matemáticas Aplicadas'),
('Biología Molecular'),
('Química Orgánica');

-- Insertar asignaturas
INSERT INTO asignatura (nombre, creditos, tipo, curso, cuatrimestre, id_profesor, id_grado) VALUES
('Programación', 6.0, 'obligatoria', 1, 1, 1, 1),
('Cálculo', 6.0, 'obligatoria', 1, 1, 2, 2),
('Genética', 6.0, 'obligatoria', 1, 1, 3, 3),
('Química Orgánica I', 6.0, 'obligatoria', 1, 1, 4, 4);

-- Insertar cursos escolares
INSERT INTO curso_escolar (anyo_inicio, anyo_fin) VALUES
(2023, 2024),
(2022, 2023),
(2021, 2022);

-- Insertar matriculaciones de alumnos en asignaturas
INSERT INTO alumno_se_matricula_asignatura (id_alumno, id_asignatura, id_curso_escolar) VALUES
(1, 1, 1),
(2, 2, 1),
(3, 3, 1),
(4, 4, 1);

-- Insertar ciudades adicionales
INSERT INTO ciudad (nombre) VALUES
('Málaga'),
('Bilbao'),
('Alicante'),
('Granada');

-- Insertar departamentos adicionales
INSERT INTO departamento (nombre) VALUES
('Física'),
('Historia'),
('Arquitectura'),
('Economía');

-- Insertar direcciones de alumnos adicionales
INSERT INTO direccionAlumno (nombre, id_ciudad) VALUES
('Calle E', 5),
('Calle F', 6),
('Calle G', 7),
('Calle H', 8);

-- Insertar direcciones de profesores adicionales
INSERT INTO direccionProfesor (nombre, id_ciudad) VALUES
('Av. P', 5),
('Av. Q', 6),
('Av. R', 7),
('Av. S', 8);

-- Insertar alumnos adicionales
INSERT INTO alumno (nif, nombre, apellido1, apellido2, fecha_nacimiento, sexo, id_direccion) VALUES
('34567890E', 'Laura', 'García', 'Sánchez', '1994-07-18', 'M', 5),
('67890123F', 'David', 'Ruiz', 'Martínez', '1990-11-30', 'H', 6),
('90123456G', 'Elena', 'López', 'Hernández', '1993-04-25', 'M', 7),
('23456789H', 'Sergio', 'Fernández', 'Gutiérrez', '1992-01-10', 'H', 8);

-- Insertar profesores adicionales
INSERT INTO profesor (nif, nombre, apellido1, apellido2, fecha_nacimiento, sexo, id_departamento, id_direccion) VALUES
('78901234I', 'Carmen', 'Sanz', 'Martín', '1983-08-05', 'M', 1, 5),
('89012345J', 'Javier', 'Gómez', 'Fernández', '1978-03-15', 'H', 2, 6),
('01234567K', 'Nuria', 'Hernández', 'López', '1980-12-20', 'M', 3, 7),
('12345678L', 'Daniel', 'Martínez', 'Pérez', '1975-09-28', 'H', 4, 8);

-- Insertar teléfonos de alumnos adicionales
INSERT INTO telefonoAlumno (numero, tipo, id_alumno) VALUES
('444555666', 'casa', 5),
('777888999', 'celular', 6),
('000111222', 'trabajo', 7),
('333444555', 'casa', 8);

-- Insertar teléfonos de profesores adicionales
INSERT INTO telefonoProfesor (numero, tipo, id_profesor) VALUES
('222333444', 'casa', 5),
('555666777', 'celular', 6),
('888999000', 'trabajo', 7),
('111222333', 'casa', 8);

-- Insertar grados adicionales
INSERT INTO grado (nombre) VALUES
('Arquitectura Técnica'),
('Historia del Arte'),
('Economía Empresarial'),
('Física Teórica');

-- Insertar asignaturas adicionales
INSERT INTO asignatura (nombre, creditos, tipo, curso, cuatrimestre, id_profesor, id_grado) VALUES
('Física Cuántica', 6.0, 'obligatoria', 1, 1, 5, 4),
('Arte Contemporáneo', 6.0, 'obligatoria', 1, 1, 6, 2),
('Microeconomía', 6.0, 'obligatoria', 1, 1, 7, 3),
('Diseño Arquitectónico', 6.0, 'obligatoria', 1, 1, 8, 1);

-- Insertar cursos escolares adicionales
INSERT INTO curso_escolar (anyo_inicio, anyo_fin) VALUES
(2020, 2021),
(2019, 2020),
(2018, 2019);

Consultas sobre una tabla
1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de
todos los alumnos. El listado deberá estar ordenado alfabéticamente de
menor a mayor por el primer apellido, segundo apellido y nombre.

SELECT apellido1, apellido2, nombre
FROM alumno
ORDER BY apellido1, apellido2, nombre;

+------------+------------+--------+
| apellido1  | apellido2  | nombre |
+------------+------------+--------+
| Fernández  | Gutiérrez  | Sergio |
| García     | Sánchez    | Laura  |
| López      | Fernández  | María  |
| López      | Hernández  | Elena  |
| Martínez   | García     | Ana    |
| Pérez      | Gómez      | Juan   |
| Ruiz       | Martínez   | David  |
| Sánchez    | Rodríguez  | Pedro  |
+------------+------------+--------+


2. Averigua el nombre y los dos apellidos de los alumnos que no han dado de
alta su número de teléfono en la base de datos.

SELECT nombre, apellido1, apellido2
FROM alumno
LEFT JOIN telefonoAlumno ON alumno.id = telefonoAlumno.id_alumno
WHERE telefonoAlumno.id IS NULL;
Empty set (0.01 sec)

3. Devuelve el listado de los alumnos que nacieron en 1999.

SELECT nombre 
FROM alumno
WHERE YEAR(fecha_nacimiento) = 1999;
Empty set (0.04 sec)

4. Devuelve el listado de profesores que no han dado de alta su número de
teléfono en la base de datos y además su nif termina en K.

SELECT nombre 
FROM profesor
LEFT JOIN telefonoProfesor ON profesor.id = telefonoProfesor.id_profesor
WHERE telefonoProfesor.id IS NULL AND profesor.nif LIKE '%k';
Empty set (0.00 sec)

5. Devuelve el listado de las asignaturas que se imparten en el primer
cuatrimestre, en el tercer curso del grado que tiene el identificador 7.

SELECT asignatura.nombre 
FROM asignatura 
JOIN grado ON asignatura.id_grado = grado.id 
WHERE grado.id = 7 
AND asignatura.cuatrimestre = 1 
AND asignatura.curso = 3;

Empty set (0.03 sec)


Consultas multitabla (Composición interna)

1. Devuelve un listado con los datos de todas las alumnas que se han
matriculado alguna vez en el Grado en Ingeniería Informática (Plan 2015).

SELECT alumno.id, alumno.nif, alumno.nombre, alumno.apellido1, alumno.apellido2
FROM alumno
JOIN alumno_se_matricula_asignatura ON alumno.id = alumno_se_matricula_asignatura.id_alumno
JOIN asignatura ON alumno_se_matricula_asignatura.id_asignatura = asignatura.id
JOIN grado ON asignatura.id_grado = grado.id
WHERE grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)' AND alumno.sexo = 'M';
Empty set (0.01 sec)




2. Devuelve un listado con todas las asignaturas ofertadas en el Grado en
Ingeniería Informática (Plan 2015).

SELECT nombre.asignatura 
FROM asignatura 
JOIN grado ON asignatura.id_grado = grado.id
WHERE grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)';




3. Devuelve un listado de los profesores junto con el nombre del
departamento al que están vinculados. El listado debe devolver cuatro
columnas, primer apellido, segundo apellido, nombre y nombre del
departamento. El resultado estará ordenado alfabéticamente de menor a
mayor por los apellidos y el nombre.

SELECT p.apellido1, p.apellido2, p.nombre, d.nombre AS departamento
FROM profesor AS p
JOIN departamento AS d ON p.id_departamento = d.id
ORDER BY p.apellido1, p.apellido2, p.nombre;

+------------+------------+--------+--------------+
| apellido1  | apellido2  | nombre | departamento |
+------------+------------+--------+--------------+
| Díaz       | Sanz       | Laura  | Matemáticas  |
| Fernández  | Gómez      | Sara   | Química      |
| Gómez      | Fernández  | Javier | Matemáticas  |
| González   | Ruiz       | Carlos | Informática  |
| Hernández  | López      | Nuria  | Biología     |
| Martín     | López      | Pablo  | Biología     |
| Martínez   | Pérez      | Daniel | Química      |
| Sanz       | Martín     | Carmen | Informática  |
+------------+------------+--------+--------------+


4. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de
fin del curso escolar del alumno con nif 26902806M.

SELECT asignatura.nombre AS asignatura, curso_escolar.anyo_inicio, curso_escolar.anyo_fin
FROM asignatura
JOIN alumno_se_matricula_asignatura ON asignatura.id = alumno_se_matricula_asignatura.id_asignatura
JOIN curso_escolar ON alumno_se_matricula_asignatura.id_curso_escolar = curso_escolar.id
JOIN alumno ON alumno_se_matricula_asignatura.id_alumno = alumno.id
WHERE alumno.nif = '26902806M';

5. Devuelve un listado con el nombre de todos los departamentos que tienen
profesores que imparten alguna asignatura en el Grado en Ingeniería
Informática (Plan 2015).

SELECT DISTINCT departamento.nombre
FROM departamento
JOIN profesor ON departamento.id = profesor.id_departamento
JOIN asignatura ON profesor.id = asignatura.id_profesor
JOIN grado ON asignatura.id_grado = grado.id
WHERE grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)';
Empty set (0.01 sec)


6. Devuelve un listado con todos los alumnos que se han matriculado en
alguna asignatura durante el curso escolar 2018/2019.

SELECT DISTINCT alumno.id, alumno.nif, alumno.nombre, alumno.apellido1, alumno.apellido2
FROM alumno
JOIN alumno_se_matricula_asignatura ON alumno.id = alumno_se_matricula_asignatura.id_alumno
JOIN curso_escolar ON alumno_se_matricula_asignatura.id_curso_escolar = curso_escolar.id
WHERE curso_escolar.anyo_inicio = 2018 AND curso_escolar.anyo_fin = 2019;
Empty set (0.01 sec)



Consultas multitabla (Composición externa)
Resuelva todas las consultas utilizando las cláusulas LEFT JOIN y RIGHT JOIN.

1. Devuelve un listado con los nombres de todos los profesores y los
departamentos que tienen vinculados. El listado también debe mostrar
aquellos profesores que no tienen ningún departamento asociado. El listado
debe devolver cuatro columnas, nombre del departamento, primer apellido,
segundo apellido y nombre del profesor. El resultado estará ordenado
alfabéticamente de menor a mayor por el nombre del departamento,
apellidos y el nombre.

SELECT departamento.nombre AS nombre_departamento, profesor.apellido1, profesor.apellido2, profesor.nombre
FROM profesor
LEFT JOIN departamento ON profesor.id_departamento = departamento.id
ORDER BY departamento.nombre, profesor.apellido1, profesor.apellido2, profesor.nombre;

+---------------------+------------+------------+--------+
| nombre_departamento | apellido1  | apellido2  | nombre |
+---------------------+------------+------------+--------+
| Biología            | Hernández  | López      | Nuria  |
| Biología            | Martín     | López      | Pablo  |
| Informática         | González   | Ruiz       | Carlos |
| Informática         | Sanz       | Martín     | Carmen |
| Matemáticas         | Díaz       | Sanz       | Laura  |
| Matemáticas         | Gómez      | Fernández  | Javier |
| Química             | Fernández  | Gómez      | Sara   |
| Química             | Martínez   | Pérez      | Daniel |
+---------------------+------------+------------+--------+


2. Devuelve un listado con los profesores que no están asociados a un
departamento.

SELECT profesor.apellido1, profesor.apellido2, profesor.nombre
FROM profesor
LEFT JOIN departamento ON profesor.id_departamento = departamento.id
WHERE departamento.id IS NULL;

Empty set (0.01 sec)


3. Devuelve un listado con los departamentos que no tienen profesores
asociados.

SELECT departamento.nombre AS nombre_departamento
FROM departamento
LEFT JOIN profesor ON departamento.id = profesor.id_departamento
WHERE profesor.id IS NULL;
+---------------------+
| nombre_departamento |
+---------------------+
| Física              |
| Historia            |
| Arquitectura        |
| Economía            |
+---------------------+

4. Devuelve un listado con los profesores que no imparten ninguna asignatura.

SELECT profesor.apellido1, profesor.apellido2, profesor.nombre
FROM profesor
LEFT JOIN asignatura ON profesor.id = asignatura.id_profesor
WHERE asignatura.id IS NULL;
Empty set (0.00 sec)

5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

SELECT asignatura.nombre 
FROM asignatura
LEFT JOIN profesor ON asignatura.id_profesor = profesor.id 
WHERE profesor.id IS NULL;
Empty set (0.01 sec)

6. Devuelve un listado con todos los departamentos que tienen alguna
asignatura que no se haya impartido en ningún curso escolar. El resultado
debe mostrar el nombre del departamento y el nombre de la asignatura que
no se haya impartido nunca.

SELECT departamento.nombre AS nombre_departamento, asignatura.nombre AS nombre_asignatura
FROM departamento
LEFT JOIN profesor ON departamento.id = profesor.id_departamento
LEFT JOIN asignatura ON profesor.id_departamento = asignatura.id
LEFT JOIN alumno_se_matricula_asignatura ama ON asignatura.id = ama.id_asignatura
WHERE ama.id_curso_escolar IS NULL;

+---------------------+-------------------+
| nombre_departamento | nombre_asignatura |
+---------------------+-------------------+
| Física              | NULL              |
| Historia            | NULL              |
| Arquitectura        | NULL              |
| Economía            | NULL              |
+---------------------+-------------------+



Consultas resumen
1. Devuelve el número total de alumnas que hay.
SELECT COUNT(*) AS numeroAlumnas
FROM alumno
WHERE sexo = 'M';

+---------------+
| numeroAlumnas |
+---------------+
|             4 |
+---------------+

2. Calcula cuántos alumnos nacieron en 1999.

SELECT COUNT(*) AS nacidos1999
FROM alumno
WHERE YEAR(fecha_nacimiento)  = 1999;

+-------------+
| nacidos1999 |
+-------------+
|           0 |
+-------------+

3. Calcula cuántos profesores hay en cada departamento. El resultado sólo
debe mostrar dos columnas, una con el nombre del departamento y otra
con el número de profesores que hay en ese departamento. El resultado
sólo debe incluir los departamentos que tienen profesores asociados y
deberá estar ordenado de mayor a menor por el número de profesores.

SELECT COUNT(profesor.id) AS profesores, departamento.nombre AS departamento
FROM departamento
RIGHT JOIN profesor ON profesor.id_departamento = departamento.id
GROUP BY departamento.nombre
ORDER BY profesores DESC;
+------------+--------------+
| profesores | departamento |
+------------+--------------+
|          2 | Informática  |
|          2 | Matemáticas  |
|          2 | Biología     |
|          2 | Química      |
+------------+--------------+





4. Devuelve un listado con todos los departamentos y el número de profesores
que hay en cada uno de ellos. Tenga en cuenta que pueden existir
departamentos que no tienen profesores asociados. Estos departamentos
también tienen que aparecer en el listado.

SELECT COUNT(profesor.id) AS profesores, departamento.nombre AS departamento
FROM departamento
LEFT JOIN profesor ON profesor.id_departamento = departamento.id
GROUP BY departamento.nombre;

+------------+--------------+
| profesores | departamento |
+------------+--------------+
|          2 | Informática  |
|          2 | Matemáticas  |
|          2 | Biología     |
|          2 | Química      |
|          0 | Física       |
|          0 | Historia     |
|          0 | Arquitectura |
|          0 | Economía     |
+------------+--------------+

5. Devuelve un listado con el nombre de todos los grados existentes en la base
de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta
que pueden existir grados que no tienen asignaturas asociadas. Estos grados
también tienen que aparecer en el listado. El resultado deberá estar
ordenado de mayor a menor por el número de asignaturas.

SELECT g.nombre AS nombre_grado, COUNT(a.id) AS numero_asignaturas
FROM grado g
LEFT JOIN asignatura a ON g.id = a.id_grado
GROUP BY g.nombre
ORDER BY numero_asignaturas DESC;

+--------------------------+--------------------+
| nombre_grado             | numero_asignaturas |
+--------------------------+--------------------+
| Ingeniería Informática   |                  2 |
| Matemáticas Aplicadas    |                  2 |
| Biología Molecular       |                  2 |
| Química Orgánica         |                  2 |
| Arquitectura Técnica     |                  0 |
| Historia del Arte        |                  0 |
| Economía Empresarial     |                  0 |
| Física Teórica           |                  0 |
+--------------------------+--------------------+

6. Devuelve un listado con el nombre de todos los grados existentes en la base
de datos y el número de asignaturas que tiene cada uno, de los grados que
tengan más de 40 asignaturas asociadas.

SELECT g.nombre AS nombre_grado, COUNT(a.id) AS numero_asignaturas
FROM grado g
LEFT JOIN asignatura a ON g.id = a.id_grado
GROUP BY g.nombre
HAVING COUNT(a.id) > 40;
Empty set (0.00 sec)

7. Devuelve un listado que muestre el nombre de los grados y la suma del
número total de créditos que hay para cada tipo de asignatura. El resultado
debe tener tres columnas: nombre del grado, tipo de asignatura y la suma
de los créditos de todas las asignaturas que hay de ese tipo. Ordene el
resultado de mayor a menor por el número total de crédidos.

SELECT grado.nombre, asignatura.tipo, SUM(asignatura.creditos)
FROM asignatura
RIGHT JOIN grado ON asignatura.id_grado = grado.id
GROUP BY grado.nombre, asignatura.tipo
ORDER BY SUM(asignatura.creditos) DESC;

+--------------------------+-------------+--------------------------+
| nombre                   | tipo        | SUM(asignatura.creditos) |
+--------------------------+-------------+--------------------------+
| Ingeniería Informática   | obligatoria |                       12 |
| Matemáticas Aplicadas    | obligatoria |                       12 |
| Biología Molecular       | obligatoria |                       12 |
| Química Orgánica         | obligatoria |                       12 |
| Arquitectura Técnica     | NULL        |                     NULL |
| Historia del Arte        | NULL        |                     NULL |
| Economía Empresarial     | NULL        |                     NULL |
| Física Teórica           | NULL        |                     NULL |
+--------------------------+-------------+--------------------------+

8. Devuelve un listado que muestre cuántos alumnos se han matriculado de
alguna asignatura en cada uno de los cursos escolares. El resultado deberá
mostrar dos columnas, una columna con el año de inicio del curso escolar y
otra con el número de alumnos matriculados.

SELECT curso_escolar.anyo_inicio, COUNT(DISTINCT alumno_se_matricula_asignatura.id_alumno)
FROM curso_escolar 
LEFT JOIN alumno_se_matricula_asignatura ON curso_escolar.id = alumno_se_matricula_asignatura.id_curso_escolar
GROUP BY curso_escolar.anyo_inicio;

+-------------+----------------------------------------------------------+
| anyo_inicio | COUNT(DISTINCT alumno_se_matricula_asignatura.id_alumno) |
+-------------+----------------------------------------------------------+
|        2018 |                                                        0 |
|        2019 |                                                        0 |
|        2020 |                                                        0 |
|        2021 |                                                        0 |
|        2022 |                                                        0 |
|        2023 |                                                        4 |
+-------------+----------------------------------------------------------+

9. Devuelve un listado con el número de asignaturas que imparte cada
profesor. El listado debe tener en cuenta aquellos profesores que no
imparten ninguna asignatura. El resultado mostrará cinco columnas: id,
nombre, primer apellido, segundo apellido y número de asignaturas. El
resultado estará ordenado de mayor a menor por el número de asignaturas.

SELECT profesor.id, profesor.nombre, profesor.apellido1, profesor.apellido2, COUNT(DISTINCT asignatura.id) AS numero_asignaturas
FROM profesor
LEFT JOIN asignatura ON profesor.id = asignatura.id_profesor
GROUP BY profesor.id, profesor.nombre, profesor.apellido1, profesor.apellido2
ORDER BY numero_asignaturas DESC;

+----+--------+------------+------------+--------------------+
| id | nombre | apellido1  | apellido2  | numero_asignaturas |
+----+--------+------------+------------+--------------------+
|  1 | Carlos | González   | Ruiz       |                  1 |
|  2 | Laura  | Díaz       | Sanz       |                  1 |
|  3 | Pablo  | Martín     | López      |                  1 |
|  4 | Sara   | Fernández  | Gómez      |                  1 |
|  5 | Carmen | Sanz       | Martín     |                  1 |
|  6 | Javier | Gómez      | Fernández  |                  1 |
|  7 | Nuria  | Hernández  | López      |                  1 |
|  8 | Daniel | Martínez   | Pérez      |                  1 |
+----+--------+------------+------------+--------------------+


Subconsultas

1. Devuelve todos los datos del alumno más joven.

SELECT id, nif, nombre, apellido1, apellido2, fecha_nacimiento, sexo, id_direccion
FROM alumno
WHERE fecha_nacimiento = (
    SELECT MAX(fecha_nacimiento)
    FROM alumno
);

+----+-----------+--------+-----------+-----------+------------------+------+--------------+
| id | nif       | nombre | apellido1 | apellido2 | fecha_nacimiento | sexo | id_direccion |
+----+-----------+--------+-----------+-----------+------------------+------+--------------+
|  5 | 34567890E | Laura  | García    | Sánchez   | 1994-07-18       | M    |            5 |
+----+-----------+--------+-----------+-----------+------------------+------+--------------+


2. Devuelve un listado con los profesores que no están asociados a un departamento.

SELECT nombre
FROM profesor
WHERE id_departamento NOT IN (SELECT id FROM departamento);
Empty set (0.00 sec)

3. Devuelve un listado con los departamentos que no tienen profesores asociados.

SELECT nombre 
FROM departamento
WHERE id NOT IN (SELECT id_departamento FROM profesor);

+--------------+
| nombre       |
+--------------+
| Física       |
| Historia     |
| Arquitectura |
| Economía     |
+--------------+


4. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.

SELECT nombre
FROM profesor
WHERE id IN (SELECT id FROM departamento) AND id NOT IN (SELECT id_profesor FROM asignatura);
Empty set (0.00 sec)

5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

SELECT nombre 
FROM asignatura
WHERE id_profesor NOT IN (SELECT id FROM profesor);
Empty set (0.00 sec)

6. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.

SELECT id, nombre
FROM departamento
WHERE id NOT IN (
    SELECT DISTINCT d.id
    FROM departamento d
    INNER JOIN profesor p ON d.id = p.id_departamento
    INNER JOIN asignatura a ON p.id = a.id_profesor
);
+----+--------------+
| id | nombre       |
+----+--------------+
|  5 | Física       |
|  6 | Historia     |
|  7 | Arquitectura |
|  8 | Economía     |
+----+--------------+

VISTAS
Claro, puedo ayudarte a crear algunas vistas simples basadas en las tablas que has creado. Aquí tienes diez vistas que podrían ser útiles:

1. Vista de información básica de alumnos:

CREATE VIEW vista_alumnos AS
SELECT id, nombre, apellido1, apellido2, fecha_nacimiento, sexo
FROM alumno;


2. Vista de información básica de profesores:

CREATE VIEW vista_profesores AS
SELECT id, nombre, apellido1, apellido2, fecha_nacimiento, sexo
FROM profesor;


3. Vista de direcciones de alumnos con nombre de ciudad:
CREATE VIEW vista_direcciones_alumnos AS
SELECT da.id, da.nombre AS direccion, c.nombre AS ciudad
FROM direccionAlumno da
INNER JOIN ciudad c ON da.id_ciudad = c.id;


4. Vista de direcciones de profesores con nombre de ciudad:

CREATE VIEW vista_direcciones_profesores AS
SELECT dp.id, dp.nombre AS direccion, c.nombre AS ciudad
FROM direccionProfesor dp
INNER JOIN ciudad c ON dp.id_ciudad = c.id;


5. Vista de teléfonos de alumnos con nombre y tipo de teléfono:

CREATE VIEW vista_telefonos_alumnos AS
SELECT ta.id, ta.numero, ta.tipo, a.nombre AS alumno
FROM telefonoAlumno ta
INNER JOIN alumno a ON ta.id_alumno = a.id;


6. Vista de teléfonos de profesores con nombre y tipo de teléfono:

CREATE VIEW vista_telefonos_profesores AS
SELECT tp.id, tp.numero, tp.tipo, p.nombre AS profesor
FROM telefonoProfesor tp
INNER JOIN profesor p ON tp.id_profesor = p.id;


7. Vista de asignaturas con nombre de profesor:

CREATE VIEW vista_asignaturas_con_profesor AS
SELECT a.id, a.nombre, a.creditos, a.tipo, a.curso, a.cuatrimestre, p.nombre AS profesor
FROM asignatura a
LEFT JOIN profesor p ON a.id_profesor = p.id;


8. Vista de asignaturas por grado:

CREATE VIEW vista_asignaturas_por_grado AS
SELECT a.id, a.nombre AS asignatura, g.nombre AS grado
FROM asignatura a
INNER JOIN grado g ON a.id_grado = g.id;


9. Vista de alumnos matriculados en asignaturas con información básica:
CREATE VIEW vista_matriculas_alumnos AS
SELECT m.id_alumno, m.id_asignatura, m.id_curso_escolar, a.nombre AS alumno, asig.nombre AS asignatura
FROM alumno_se_matricula_asignatura m
INNER JOIN alumno a ON m.id_alumno = a.id
INNER JOIN asignatura asig ON m.id_asignatura = asig.id;


10. Vista de cursos escolares activos:

CREATE VIEW vista_cursos_escolares_activos AS
SELECT id, anyo_inicio, anyo_fin
FROM curso_escolar
WHERE anyo_fin >= YEAR(NOW());


PROCEDIMIENTOS ALMACENADOS

1. Procedimiento para crear un nuevo departamento:

DELIMITER //
CREATE PROCEDURE crear_departamento(
    IN nombre_departamento VARCHAR(50)
)
BEGIN
    INSERT INTO departamento (nombre) VALUES (nombre_departamento);
END //
DELIMITER ;


2. Procedimiento para actualizar el nombre de un departamento:

DELIMITER //
CREATE PROCEDURE actualizar_nombre_departamento(
    IN id_departamento INT,
    IN nuevo_nombre_departamento VARCHAR(50)
)
BEGIN
    UPDATE departamento SET nombre = nuevo_nombre_departamento WHERE id = id_departamento;
END //
DELIMITER ;


3. Procedimiento para eliminar un departamento y sus profesores asociados:

DELIMITER //
CREATE PROCEDURE eliminar_departamento(
    IN id_departamento INT
)
BEGIN
    DELETE FROM profesor WHERE id_departamento = id_departamento;
    DELETE FROM departamento WHERE id = id_departamento;
END //
DELIMITER ;


4. Procedimiento para buscar un alumno por su NIF:

DELIMITER //
CREATE PROCEDURE buscar_alumno_por_nif(
    IN nif_alumno VARCHAR(9)
)
BEGIN
    SELECT * FROM alumno WHERE nif = nif_alumno;
END //
DELIMITER ;


5. Procedimiento para buscar un profesor por su NIF:

DELIMITER //
CREATE PROCEDURE buscar_profesor_por_nif(
    IN nif_profesor VARCHAR(9)
)
BEGIN
    SELECT * FROM profesor WHERE nif = nif_profesor;
END //
DELIMITER ;


6. Procedimiento para crear una nueva asignatura:

DELIMITER //
CREATE PROCEDURE crear_asignatura(
    IN nombre_asignatura VARCHAR(100),
    IN creditos FLOAT,
    IN tipo_asignatura ENUM('básica', 'obligatoria', 'optativa'),
    IN curso TINYINT,
    IN cuatrimestre TINYINT,
    IN id_profesor INT,
    IN id_grado INT
)
BEGIN
    INSERT INTO asignatura (nombre, creditos, tipo, curso, cuatrimestre, id_profesor, id_grado)
    VALUES (nombre_asignatura, creditos, tipo_asignatura, curso, cuatrimestre, id_profesor, id_grado);
END //
DELIMITER ;

7. **Procedimiento para actualizar el nombre de un alumno:**

DELIMITER //
CREATE PROCEDURE actualizar_nombre_alumno(
    IN id_alumno INT,
    IN nuevo_nombre VARCHAR(25)
)
BEGIN
    UPDATE alumno SET nombre = nuevo_nombre WHERE id = id_alumno;
END //
DELIMITER ;


8. Procedimiento para eliminar un teléfono de alumno:

DELIMITER //
CREATE PROCEDURE eliminar_telefono_alumno(
    IN id_telefono INT
)
BEGIN
    DELETE FROM telefonoAlumno WHERE id = id_telefono;
END //
DELIMITER ;


9. Procedimiento para buscar una asignatura por su nombre:

DELIMITER //
CREATE PROCEDURE buscar_asignatura_por_nombre(
    IN nombre_asignatura VARCHAR(100)
)
BEGIN
    SELECT * FROM asignatura WHERE nombre = nombre_asignatura;
END //
DELIMITER ;


10. Procedimiento para buscar alumnos matriculados en una asignatura:

DELIMITER //
CREATE PROCEDURE buscar_alumnos_por_asignatura(
    IN id_asignatura INT
)
BEGIN
    SELECT a.* FROM alumno a
    INNER JOIN alumno_se_matricula_asignatura m ON a.id = m.id_alumno
    WHERE m.id_asignatura = id_asignatura;
END //
DELIMITER ;


