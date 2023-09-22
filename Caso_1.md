## DESARROLLO DEL CASO TIENDA RETAIL

Existe un local de artículos electrónicos que vende insumos y repuestos para Televisores y Computadores, al interior del local se maneja una base de datos sencilla donde diariamente se deben generar reportes de ventas efectuadas en la semana o mes en curso.

Su misión consiste en diseñar una base de datos pequeña y simple para satisfacer los requerimientos más importantes que se requieren, Para esto, el gerente del local le entrega un esquema de la información en Excel, que usted deberá optimizar mediante SQL para programar algunas tareas claves.

El siguiente es un esquema resumido de parte de la información y datos que se manejan:

### clientes

| rut_cli    | nombre_cliente | direccion_cliente                |
| ---------- | -------------- | -------------------------------- |
| 13457305-9 | Ana Castro     | AV.Carmen 1680 Recoleta          |
| 20242591-K | José Mora      | Alonso Ovalle 240 La Reina       |
| 18357596-6 | Luis Parodi    | Arturo Prat 4410 Santiago Centro |

### boletas

| folio | fecha      | total | forma_pago | rut_cli    |
| ----- | ---------- | ----- | ---------- | ---------- |
| 3456  | 2011-09-08 | 70000 | Debito     | 20242591-K |
| 3457  | 2021-07-08 | 60000 | Efectivo   | 13457305-9 |
| 3458  | 2022-07-18 | 25000 | Efectivo   | 13457305-9 |
| 3459  | 2005-05-04 | 80000 | Debito     | 20242591-K |
| 3460  | 2021-07-08 | 70000 | Efectivo   | 20242591-K |

## PREGUNTAS:

#### 1. Usted debe crear las entidades Cliente y Boleta definiendo las llaves primarias y foráneas, para luego insertar los datos a las entidades tal como se describen en la imagen.

### Crear la base de datos y las tablas en MySQL

```sql
CREATE DATABASE IF NOT EXISTS bd_tienda_retail;

USE bd_tienda;

CREATE TABLE clientes(
  rut_cli CHAR(10) PRIMARY KEY,
  nombre_cliente VARCHAR(50) NOT NULL UNIQUE,
  direccion_cliente VARCHAR(100)
);

CREATE TABLE boletas(
  folio INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  fecha DATE NOT NULL,
  total DECIMAL(10,2) NOT NULL,
  forma_pago ENUM('Efectivo', 'Debito', 'Credito') NOT NULL,
  rut_cli CHAR(10),
  FOREIGN KEY(rut_cli) REFERENCES clientes(rut_cli)
)AUTO_INCREMENT = 3461;
```

### Insertar datos en MySQL

```sql
INSERT INTO clientes (rut_cli, nombre_cliente, direccion_cliente)
VALUE
('13457305-9', 'Ana Castro', 'AV.Carmen 1680 Recoleta'),
('20242591-K', 'José Mora', 'Alonso Ovalle 240 La Reina'),
('18357596-6', 'Luis Parodi', 'Arturo Prat 4410 Santiago Centro');

INSERT INTO boletas (folio, fecha, total, forma_pago, rut_cli)
VALUE
(3456, '2011-09-08', 70000, 2, '20242591-K'),
(3457, '2021-07-08', 60000, 1, '13457305-9'),
(3458, '2022-07-18', 25000, 1, '13457305-9'),
(3459, '2005-05-04', 80000, 2, '20242591-K'),
(3460, '2021-07-08', 70000, 1, '20242591-K');
```

#### 2. Desarrollar las siguientes consultas:

a) Crear una consulta o query mediante el comando select que permita obtener el nombre del cliente, dirección, fecha de las boletas y forma de pago de aquellas transacciones realizadas con la forma de pago DEBITO.

```sql
SELECT nombre_cliente, direccion_cliente, fecha, forma_pago
FROM clientes, boletas
WHERE forma_pago = 'Debito';
```

b) Obtener el promedio de todas las boletas realizadas por el cliente José Mora.

```sql
SELECT c.nombre_cliente, COUNT(*) AS Promedio
FROM boletas b
JOIN clientes c ON b.rut_cli = c.rut_cli
WHERE c.nombre_cliente = 'José Mora';
```

#### 3. Implementar una vista llamada Vista reporte, que contenga el Rut y nombre del cliente con la respectiva identificación de la boleta como el folio, fecha, total y forma de pago de todas las transacciones realizadas entre los años 2020 y 2022 ordenadas por Rut del cliente.

```sql
CREATE OR REPLACE VIEW vista_reporte
AS SELECT c.rut_cli, c.nombre_cliente, b.folio, b.fecha, b.total, b.forma_pago
FROM boletas b
JOIN clientes c ON b.rut_cli = c.rut_cli
WHERE b.fecha BETWEEN '2020-01-01' AND '2022-12-31' ORDER BY c.rut_cli ASC;
```
