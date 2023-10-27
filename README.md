# Proyecto Final
### Empresa Web

------------


### Integrantes:
- Elzer David Ordoñez Solorzano, 7691-13-18997
- Diego Fernando Velásquez Pichilla, 7690-16-3882
- Ana María Montufar Aguirre, 7691-18-26253
- Luis Enrique Sazo Rosales, 7691-21-1778
- Cristhian Alejandro Barrientos Moya, 7691-20-3889
- Guillermo Sebastian Chamalé Flores, 7691-21-7580

------------


## Lineamientos:
1. Crear una base de datos en **mysql**

2. Crear el mantenimiento web (CRUD) de la tabla **Puestos**.

3. Crear el mantenimiento web (CRUD) de la tabla **Empleados** la cual deberá de mostrar un combo con los **puestos** de la tabla **Puestos** y un hipervínculo que redirecciones al mantenimiento de **Puestos** y viceversa.

4. Crear el mantenimiento web (CRUD) de la tabla **Clientes**.

5. Crear el mantenimiento web (CRUD) de la tabla **Proveedores**.

6. Crear el mantenimiento web (CRUD) de la tabla **Marcas**.

7. Crear el mantenimiento web (CRUD) de la tabla **Productos** el cual deberá de mostrar un combo con las **marcas** de la tabla **Marcas** y un hipervínculo que redirecciones al mantenimiento de **Marcas** y viceversa. Este mantenimiento deberá de permitir Guardar una **IMAGEN** del producto en el servidor,(no en la base de datos ahí solo deberá de estar la URL de la IMAGEN) y cuando se realice una búsqueda del producto esta deberá demostrar la imagen almacenada.

8. Crear un mantenimiento web (CRUD) de tipo **MAESTRO DETALLE** de las tablas **Ventas y Ventas_Detalle**, es decir en un solo mantenimiento se deberá de guardar en las dos tablas. El mantenimiento deberá de mostrar un combo con los nombres y nit de los **clientes** de la tabla **Clientes** y un hipervínculo que redirecciones al mantenimiento de **Clientes** y viceversa. El mantenimiento deberá de mostrar un combo con los nombres de los **empleados** de la tabla **Empleados** y un hipervínculo que redirecciones al mantenimiento de **Empleados** y viceversa. Cuando se ingrese una venta el saldo del producto de la tabla **Producto** deberá de disminuir.

9. Crear un mantenimiento web (CRUD) de tipo **MAESTRO DETALLE** de las tablas **Compras y Compras_Detalle**, es decir en un solo mantenimiento se deberá de guardar en las dos tablas. El mantenimiento deberá de mostrar un combo con los nombres de los **proveedores** de la tabla **Proveedores** y un hipervínculo que redirecciones al mantenimiento de **Proveedores** y viceversa. Cuando se ingrese una compra el saldo del producto de la tabla **Producto** deberá de aumentar y el precio_costo deberá de actualizarse, así como el precio_venta pero este con un 25% más del precio_costo.

10. Deberá de crear un login para ingresar a la aplicación (Crear una tabla en la base de datos usuarios para almacenar el usuario y contraseña).

11. Deberá de crear un menú principal **DINAMICO** (Crear una tabla en la base de Datos para los menús) por medio de Arboles con la siguiente estructura.
	1. Productos
		1.1. Marcas
	2. Ventas
		2.1. Clientes
		2.2. Empleados
		2.2.1. Puestos
	3. Compras
		3.1. Proveedores
	4. Reportes

12. Todos los mantenimientos deberán de llevar las validaciones básicas para el ingreso de datos.

13. Crear como mínimo 5 reportes básicos con JasperReports u otra alternativa a su elección.

------------

## Base de datos:
```sql
create database proyecto_db;
use proyecto_db;

create table clientes(
id_cliente int primary key not null auto_increment,
nombres varchar(60),
apellidos varchar(60),
nit varchar(12),
genero bit,
telefono varchar(25),
correo_electronico varchar(45),
fecha_ingreso datetime
);

create table puestos(
id_puesto smallint not null primary key auto_increment,
puesto varchar(50)
);

create table proveedores(
id_proveedor int not null primary key auto_increment,
proveedor varchar(60),
nit varchar(12),
direccion varchar(80),
telefono varchar(25)
);

create table marcas(
id_marca smallint not null auto_increment primary key,
marca varchar(50)
);

create table empleados(
id_empleado int not null primary key auto_increment,
nombres varchar(60),
apellidos varchar(60),
direccion varchar(80),
telefono varchar(25),
dpi varchar(15),
genero bit,
fecha_nacimiento date,
id_puesto smallint not null,
fecha_inicio_labores date,
fecha_ingreso datetime,
constraint fk_empleados_id_puesto foreign key (id_puesto) references puestos(id_puesto)
);

create table productos(
id_producto int not null auto_increment primary key,
producto varchar(50),
id_marca smallint not null,
descripcion varchar(100),
imagen varchar(30),
precio_costo decimal(8,2),
precio_venta decimal(8,2),
existencia int,
fecha_ingreso datetime,
constraint fk_productos_id_marca foreign key (id_marca) references marcas(id_marca)
);

create table compras(
id_compra int not null auto_increment primary key,
no_orden_compra int,
id_proveedor int not null,
fecha_orden date,
fecha_ingreso datetime,
constraint fk_compras_id_proveedor foreign key (id_proveedor) references proveedores(id_proveedor) 
);

create table ventas(
id_venta int not null auto_increment primary key,
no_factura int,
serie char(1),
fecha_factura date,
id_cliente int not null,
id_empleado int not null,
fecha_ingreso datetime,
constraint fk_ventas_id_cliente foreign key (id_cliente) references clientes(id_cliente),
constraint fk_ventas_id_empleado foreign key (id_empleado) references empleados(id_empleado)
);

create table ventas_detalle(
id_venta_detalle bigint not null auto_increment primary key,
id_venta int not null,
id_producto int not null,
cantidad varchar(45),
precio_unitario decimal(8,2),
constraint fk_ventas_detalle_id_venta foreign key (id_venta) references ventas(id_venta),
constraint fk_ventas_detalle_id_producto foreign key (id_producto) references productos(id_producto)
);

create table compras_detalle(
id_compra_detalle bigint not null auto_increment primary key,
id_compra int not null,
id_producto int not null,
cantidad int,
precio_costo_unitario decimal(8,2),
constraint fk_compras_detalle_id_compra foreign key (id_compra) references compras(id_compra),
constraint fk_compras_detalle_id_producto foreign key (id_producto) references productos(id_producto)
);
```

------------

### Datos para Pruebas:
```sql
-- Inserción de clientes//
INSERT INTO clientes(nombres, apellidos, nit, genero, telefono, correo_electronico, fecha_ingreso)
VALUES('Juan', 'Gómez', '1234567890', 1, '555-123-456', 'juan@gmail.com', '2022-09-01 10:00:00');

INSERT INTO clientes(nombres, apellidos, nit, genero, telefono, correo_electronico, fecha_ingreso)
VALUES('María', 'López', '9876543210', 0, '555-789-012', 'maria@hotmail.com', '2022-08-15 14:30:00');

INSERT INTO clientes(nombres, apellidos, nit, genero, telefono, correo_electronico, fecha_ingreso)
VALUES('Pedro', 'Martínez', '5432167890', 1, '555-234-567', 'pedro@yahoo.com', '2022-07-20 09:15:00');

INSERT INTO clientes(nombres, apellidos, nit, genero, telefono, correo_electronico, fecha_ingreso)
VALUES('Laura', 'González', '9876123450', 0, '555-678-901', 'laura@gmail.com', '2022-06-05 16:45:00');

INSERT INTO clientes(nombres, apellidos, nit, genero, telefono, correo_electronico, fecha_ingreso)
VALUES('Carlos', 'Rodríguez', '1122334455', 1, '555-345-678', 'carlos@hotmail.com', '2022-05-10 08:30:00');

-- Inserción de puestos//
INSERT INTO puestos(puesto)
VALUES('Gerente');

INSERT INTO puestos(puesto)
VALUES('Programador Senior');

INSERT INTO puestos(puesto)
VALUES('Diseñador Gráfico');

INSERT INTO puestos(puesto)
VALUES('Asistente de Ventas');

INSERT INTO puestos(puesto)
VALUES('Analista de Datos');

-- Inserción de proveedores//
INSERT INTO proveedores(proveedor, nit, direccion, telefono)
VALUES('Suministros Electrónicos S.A.', '1234567890', 'Calle Tecnológica 123, Ciudad X', '555-123-456');

INSERT INTO proveedores(proveedor, nit, direccion, telefono)
VALUES('Materiales Industriales Ltda.', '9876543210', 'Avenida Industrial 456, Ciudad Y', '555-789-012');

INSERT INTO proveedores(proveedor, nit, direccion, telefono)
VALUES('Componentes Electrónicos Globales', '5432167890', 'Calle de la Innovación 789, Ciudad Z', '555-234-567');

INSERT INTO proveedores(proveedor, nit, direccion, telefono)
VALUES('Productos Tecnológicos Avanzados', '9876123450', 'Avenida de la Tecnología 101, Ciudad W', '555-678-901');

INSERT INTO proveedores(proveedor, nit, direccion, telefono)
VALUES('Electrónica Moderna S.A.', '1122334455', 'Calle Innovadora 555, Ciudad V', '555-345-678');

-- Inserción de marcas//
INSERT INTO marcas(marca)
VALUES('SONY');

INSERT INTO marcas(marca)
VALUES('LG');

INSERT INTO marcas(marca)
VALUES('DELL');

INSERT INTO marcas(marca)
VALUES('Toshiba');

INSERT INTO marcas(marca)
VALUES('SAMSUNG');

-- Inserción de empleados//
INSERT INTO empleados(nombres, apellidos, direccion, telefono, dpi, genero, fecha_nacimiento, id_puesto, fecha_inicio_labores, fecha_ingreso)
VALUES('Juan', 'Gómez', 'Calle 123, Ciudad X', '555-123-456', '123456789012345', 1, '1990-05-15', 1, '2020-07-01', '2020-07-01 09:00:00');

INSERT INTO empleados(nombres, apellidos, direccion, telefono, dpi, genero, fecha_nacimiento, id_puesto, fecha_inicio_labores, fecha_ingreso)
VALUES('María', 'López', 'Avenida Principal, Ciudad Y', '555-789-012', '987654321098765', 0, '1985-08-20', 2, '2019-09-15', '2019-09-15 10:30:00');

INSERT INTO empleados(nombres, apellidos, direccion, telefono, dpi, genero, fecha_nacimiento, id_puesto, fecha_inicio_labores, fecha_ingreso)
VALUES('Pedro', 'Martínez', 'Calle 456, Ciudad Z', '555-234-567', '543216789054321', 1, '1992-03-10', 3, '2021-02-01', '2021-02-01 11:15:00');

INSERT INTO empleados(nombres, apellidos, direccion, telefono, dpi, genero, fecha_nacimiento, id_puesto, fecha_inicio_labores, fecha_ingreso)
VALUES('Laura', 'González', 'Avenida Secundaria, Ciudad W', '555-678-901', '987612345098761', 0, '1988-11-05', 4, '2018-10-15', '2018-10-15 14:45:00');

INSERT INTO empleados(nombres, apellidos, direccion, telefono, dpi, genero, fecha_nacimiento, id_puesto, fecha_inicio_labores, fecha_ingreso)
VALUES('Carlos', 'Rodríguez', 'Calle 789, Ciudad V', '555-345-678', '112233445511223', 1, '1995-07-25', 5, '2017-06-01', '2017-06-01 08:30:00');

-- Inserción de productos//
INSERT INTO productos(producto, id_marca, descripcion, imagen, precio_costo, precio_venta, existencia, fecha_ingreso)
VALUES('Teclado', 1, 'Teclado inalámbrico para computadora', 'teclado.jpg', 30.00, 45.00, 200, '2022-08-10 10:00:00');

INSERT INTO productos(producto, id_marca, descripcion, imagen, precio_costo, precio_venta, existencia, fecha_ingreso)
VALUES('Pantalla', 2, 'Pantalla LED de 24 pulgadas', 'pantalla.jpg', 120.00, 160.00, 100, '2022-08-15 14:30:00');

INSERT INTO productos(producto, id_marca, descripcion, imagen, precio_costo, precio_venta, existencia, fecha_ingreso)
VALUES('SSD', 4, 'Unidad de estado sólido de 256 GB', 'ssd.jpg', 50.00, 75.00, 80, '2022-09-01 09:15:00');

INSERT INTO productos(producto, id_marca, descripcion, imagen, precio_costo, precio_venta, existencia, fecha_ingreso)
VALUES('Laptop', 3, 'Laptop con procesador i5 y 8 GB de RAM', 'laptop.jpg', 500.00, 650.00, 50, '2022-09-05 16:45:00');

INSERT INTO productos(producto, id_marca, descripcion, imagen, precio_costo, precio_venta, existencia, fecha_ingreso)
VALUES('Celular', 5, 'Teléfono celular con cámara de 12 MP', 'celular.jpg', 250.00, 350.00, 150, '2022-09-10 08:30:00');

-- Inserción de compras//
INSERT INTO compras(no_orden_compra, id_proveedor, fecha_orden, fecha_ingreso)
VALUES(1001, 1, '2022-08-10', '2022-08-10 10:00:00');

INSERT INTO compras(no_orden_compra, id_proveedor, fecha_orden, fecha_ingreso)
VALUES(1002, 2, '2022-08-15', '2022-08-15 14:30:00');

INSERT INTO compras(no_orden_compra, id_proveedor, fecha_orden, fecha_ingreso)
VALUES(1003, 3, '2022-09-01', '2022-09-01 09:15:00');

INSERT INTO compras(no_orden_compra, id_proveedor, fecha_orden, fecha_ingreso)
VALUES(1004, 4, '2022-09-05', '2022-09-05 16:45:00');

INSERT INTO compras(no_orden_compra, id_proveedor, fecha_orden, fecha_ingreso)
VALUES(1005, 5, '2022-09-10', '2022-09-10 08:30:00');

-- Inserción de ventas//
INSERT INTO ventas(no_factura, serie, fecha_factura, id_cliente, id_empleado, fecha_ingreso)
VALUES(1001, 'A', '2022-08-10', 1, 1, '2022-08-10 10:00:00');

INSERT INTO ventas(no_factura, serie, fecha_factura, id_cliente, id_empleado, fecha_ingreso)
VALUES(1002, 'B', '2022-08-15', 2, 2, '2022-08-15 14:30:00');

INSERT INTO ventas(no_factura, serie, fecha_factura, id_cliente, id_empleado, fecha_ingreso)
VALUES(1003, 'A', '2022-09-01', 3, 3, '2022-09-01 09:15:00');

INSERT INTO ventas(no_factura, serie, fecha_factura, id_cliente, id_empleado, fecha_ingreso)
VALUES(1004, 'C', '2022-09-05', 4, 4, '2022-09-05 16:45:00');

INSERT INTO ventas(no_factura, serie, fecha_factura, id_cliente, id_empleado, fecha_ingreso)
VALUES(1005, 'B', '2022-09-10', 5, 5, '2022-09-10 08:30:00');

-- Inserción de detalles de ventas//
INSERT INTO ventas_detalle(id_venta, id_producto, cantidad, precio_unitario)
VALUES(1, 1, '2 unidades', 45.00);

INSERT INTO ventas_detalle(id_venta, id_producto, cantidad, precio_unitario)
VALUES(2, 2, '1 unidad', 160.00);

INSERT INTO ventas_detalle(id_venta, id_producto, cantidad, precio_unitario)
VALUES(3, 3, '5 unidades', 10.00);

INSERT INTO ventas_detalle(id_venta, id_producto, cantidad, precio_unitario)
VALUES(4, 4, '3 unidades', 650.00);

INSERT INTO ventas_detalle(id_venta, id_producto, cantidad, precio_unitario)
VALUES(5, 5, '4 unidades', 350.00);

-- Inserción de detalles de compras//
INSERT INTO compras_detalle(id_compra, id_producto, cantidad, precio_costo_unitario)
VALUES(1, 1, 10, 30.00);

INSERT INTO compras_detalle(id_compra, id_producto, cantidad, precio_costo_unitario)
VALUES(2, 2, 5, 120.00);

INSERT INTO compras_detalle(id_compra, id_producto, cantidad, precio_costo_unitario)
VALUES(3, 3, 8, 500.00);

INSERT INTO compras_detalle(id_compra, id_producto, cantidad, precio_costo_unitario)
VALUES(4, 4, 15, 50.00);

INSERT INTO compras_detalle(id_compra, id_producto, cantidad, precio_costo_unitario)
VALUES(5, 5, 12, 250.00);
```
------------
