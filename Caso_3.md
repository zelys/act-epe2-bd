## DESARROLLO DEL CASO ANFP

La ANFP, desea optimizar la gestión de los fichajes y contratos de los futbolistas amateur dado que se han detectado muchas anomalías al interior de dicha organización, para esto plantean y entregan un modelo simple de base de datos, donde se gestionan la información necesaria de los clubes y jugadores.

### equipo

- cod_equipo **VARCHAR(20) PRIMARY KEY**
- nom_equipo **VARCHAR(60)**
- fecha_fund **DATE**

### jugador

- cod_jugador **VARCHAR(50) PRIMARY KEY**
- nom_jugador **VARCHAR(100)**
- genero **ENUM('masculino', 'femenino')**
- fecha_nacimiento **DATE**

### contrato

- folio **INT PRIMARY KEY**
- fecha_inicio **DATE**
- fecha_term **DATE**
- sueldo_base **REAL**
- cod_equipo **VARCHAR(20) FOREIGN KEY**
- cod_jugador **VARCHAR(50) FOREIGN KEY**

En base al modelo expuesto, usted debe programas diferentes consultas que son requeridas por los usuarios del
sistema, como gerentes y representantes de los jugadores, para la cual usted debe desarrollar los siguientes ejercicios:

## PREGUNTAS:

#### 1. Crear la base de datos respectiva identificando los tipos de datos pertinentes y agregando 5 filas de información por tabla.

### Crear la base de datos y las tablas en MySQL.

```sql
CREATE DATABASE IF NOT EXISTS bd_anfp;

USE bd_anfp;

CREATE TABLE equipo(
  cod_equipo VARCHAR(20) NOT NULL PRIMARY KEY,
  nom_equipo VARCHAR(60) NOT NULL,
  fecha_fund DATE
);

CREATE TABLE jugador(
  cod_jugador VARCHAR(50) NOT NULL PRIMARY KEY,
  nom_jugador VARCHAR(100) NOT NULL,
  genero ENUM('masculino', 'femenino') NOT NULL,
  fecha_nacimiento DATE
);

CREATE TABLE contrato(
  folio INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  fecha_inicio DATE NOT NULL,
  fecha_term DATE NOT NULL,
  sueldo_base REAL NOT NULL,
  cod_equipo VARCHAR(20) NOT NULL,
  cod_jugador VARCHAR(50) NOT NULL
)AUTO_INCREMENT = 10001;
```

### Insertar datos en MySQL.

```sql
INSERT INTO equipo (cod_equipo, nom_equipo, fecha_fund)
VALUES
('EQ1', 'Universidad Católica', '1940-01-01'),
('EQ2', 'Unión Española ', '1942-03-15'),
('EQ3', 'Universidad de Chile', '1940-07-20'),
('EQ4', 'Colo-Colo', '1945-09-10'),
('EQ5', 'Santiago Morning', '1948-12-05');

INSERT INTO jugador (cod_jugador, nom_jugador, genero, fecha_nacimiento)
VALUES
('J1', 'Andres Gonzalez', 1, '1990-04-15'),
('J2', 'Carolina Cabello', 2, '1995-06-20'),
('J3', 'Flavia Paredes', 2, '2000-08-25'),
('J4', 'Pablo Muñoz', 1, '2005-10-30'),
('J5', 'Felipe Martínez', 1, '2010-12-10');

INSERT INTO contrato (fecha_inicio, fecha_term, sueldo_base, cod_equipo, cod_jugador)
VALUES
('2023-01-01', '2024-12-31', 2000000, 'EQ1', 'J1'),
('2023-03-01', '2024-11-30', 3000000, 'EQ2', 'J5'),
('2023-04-01', '2024-10-31', 4000000, 'EQ2', 'J4'),
('2023-06-01', '2024-09-30', 3000000, 'EQ5', 'J2'),
('2023-09-01', '2024-08-31', 2000000, 'EQ5', 'J3');
```

#### 2. Se necesita listar el nombre de todos los equipos y el nombre de los jugadores de genero femenino que han sido contratados durante el presente año (Referencia fecha inicio año 2023).

```sql
SELECT e.nom_equipo, j.nom_jugador
FROM contrato c
JOIN jugador j ON c.cod_jugador = j.cod_jugador
JOIN equipo e ON c.cod_equipo = e.cod_equipo
WHERE c.fecha_inicio BETWEEN '2023-01-01' AND '2023-12-31' AND j.genero = 2;
```

#### 3. Crear una vista o consulta que permita entregar el nombre del club y el total de jugadores que militan en dicho club.

```sql
SELECT cod_equipo, COUNT(*) AS cantidad_jugadores
FROM contrato
GROUP BY cod_equipo;
```
