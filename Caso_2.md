## DESARROLLO DEL CASO BANCO

La Superintendencia de Bancos, desea hacer un estudio de rotación del personal durante los últimos 5 años, razón por la cual existe un esquema simple de base de datos, en la que mensualmente se realizan algunas consultas.

Los bancos existentes pertenecen a un Holding privado, donde existen sucursales en diferentes sectores de la ciudad, y los empleados solo trabajan en una sucursal a menos que estén autorizados por las jefaturas superiores una rotación de su lugar de trabajo.

### BANCO

| cod_b | nom_b     |
| ----- | --------- |
| 10001 | Chase     |
| 10002 | Chile     |
| 10003 | Santander |

### SUCURSAL

| cod_s | nom_s   | cod_b |
| ----- | ------- | ----- |
| 11    | Central | 10001 |
| 12    | Sur     | 10002 |
| 13    | Florida | 10003 |

### EMPLEADO

| id_e | nom_e     | s_base  | cod_s |
| ---- | --------- | ------- | ----- |
| A1   | Alfredo   | 2500000 | 11    |
| A2   | Jose      | 1500000 | 12    |
| A3   | Miguel    | 3000000 | 12    |
| A4   | Luis      | 2000000 | 11    |
| A5   | Alejandro | 1000000 | 11    |

En base a esta estructura dada, se pide resolver las siguientes preguntas:

## PREGUNTAS:

#### 1. Crear y definir la estructura dada e insertar los registros con la información dada en la imagen. Identificando las llaves primarias y foráneas respectivas.

### Crear la base de datos y las tablas en MySQL

```sql
CREATE DATABASE IF NOT EXISTS bd_banco;

USE bd_banco;

CREATE TABLE banco(
  cod_b INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  nom_b VARCHAR(50) NOT NULL
)AUTO_INCREMENT = 10004;

CREATE TABLE sucursal(
  cod_s INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  nom_s VARCHAR(50) NOT NULL,
  cod_b INT UNSIGNED,
  FOREIGN KEY(cod_b) REFERENCES banco(cod_b)
)AUTO_INCREMENT = 14;

CREATE TABLE empleado(
  id_e CHAR(3) NOT NULL PRIMARY KEY,
  nom_e VARCHAR(50) NOT NULL,
  s_base DECIMAL(10,2) NOT NULL,
  cod_s INT UNSIGNED
);
```

### Insertar datos en MySQL

```sql
INSERT INTO banco (cod_b, nom_b)
VALUE
(10001, 'Chase'),
(10002, 'Chile'),
(10003, 'Santander');

INSERT INTO sucursal (cod_s, nom_s, cod_b)
VALUE
(11, 'Central', 10001),
(12, 'Sur', 10002),
(13, 'Florida', 10003);

INSERT INTO empleado (id_e, nom_e, s_base, cod_s)
VALUE
('A1', 'Alfredo', 2500000, 11),
('A2', 'Jose', 1500000, 12),
('A3', 'Miguel', 3000000, 12),
('A4', 'Luis', 2000000, 11),
('A5', 'Alejandro', 1000000, 11);
```

#### 2. Usted debe programar las siguientes consultas en SQL:

a) Listar el nombre de todos los bancos y los empleados respectivos ordenados por sueldo base.

```sql
SELECT b.nom_b, e.nom_e, e.s_base
FROM banco b
JOIN sucursal s ON b.cod_b = s.cod_b
JOIN empleado e ON s.cod_s = e.cod_s
ORDER BY e.s_base ASC;
```

b) Crear una consulta que permita contabilizar el total de empleados por banco.

```sql
SELECT (b.nom_b) AS banco, COUNT(*) AS cantidad_empleados
FROM banco b
JOIN sucursal s ON b.cod_b = s.cod_b
JOIN empleado e ON s.cod_s = e.cod_s
GROUP BY e.cod_s;
```

#### 3. Finalmente se necesita que, pueda determinar mediante SQL el promedio de sueldo de los empleados del banco CHASE.

```sql
SELECT (b.nom_b) AS banco, ROUND(AVG(e.s_base),0) AS Promedio
FROM banco b
JOIN sucursal s ON b.cod_b = s.cod_b
JOIN empleado e ON s.cod_s = e.cod_s
WHERE b.nom_b = 'Chase'
GROUP BY b.nom_b;
```
