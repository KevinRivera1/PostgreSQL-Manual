# Manual de PostgreSQL

Este manual proporciona una guía paso a paso para aprender PostgreSQL, un sistema de gestión de bases de datos relacional de código abierto y potente.

## Contenido

1. [Introducción](#introducción)
2. [Instalación](#instalación)
3. [Conceptos Básicos](#conceptos-básicos)
4. [Operaciones Básicas](#operaciones-básicas)
5. [Consultas de Datos](#consultas-de-datos)
6. [Modificación de Tablas](#modificación-de-tablas)
7. [Eliminación de Tablas](#eliminación-de-tablas)

## Introducción

PostgreSQL es un sistema de gestión de bases de datos relacional extremadamente potente y versátil. Se destaca por su capacidad para manejar grandes cargas de trabajo y su amplia gama de características.

## Instalación

Para instalar PostgreSQL, sigue las instrucciones específicas para tu sistema operativo en la [documentación oficial](https://www.postgresql.org/docs/).

## Conceptos Básicos

Algunos conceptos básicos que necesitas comprender antes de comenzar son:

- **Tablas:** Almacenan los datos en filas y columnas.
- **Consultas:** Se utilizan para recuperar datos de la base de datos.
- **Claves primarias y extranjeras:** Se utilizan para establecer relaciones entre tablas.
- **Comandos SQL:** Se utilizan para interactuar con la base de datos.

## Operaciones Básicas

### Creación de Tablas

```sql

-- Ejemplo de creación de tabla

-- Tabla Imagenes
CREATE TABLE Imagenes (
  id_imagen SERIAL PRIMARY KEY,
  ruta VARCHAR(255) NOT NULL,
  formato VARCHAR(10) NOT NULL,
  tamaño INT NOT NULL,
  fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabla Servicios
CREATE TABLE Servicios (
  id_servicio SERIAL PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  descripcion TEXT,
  precio DECIMAL(10,2) NOT NULL,
  proveedor VARCHAR(50) NOT NULL,
  imagenes_id INTEGER, -- Añadimos la columna imagenes_id
  CONSTRAINT fk_imagenes_id FOREIGN KEY (imagenes_id) REFERENCES imagenes(id_imagen)
);

-- Tabla Productos
CREATE TABLE Productos (
  id_producto SERIAL PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  descripcion TEXT,
  precio DECIMAL(10,2) NOT NULL,
  stock INT NOT NULL,
  marca VARCHAR(50) NOT NULL,
  dimensiones VARCHAR(50),
  imagenes_id INTEGER, -- Añadimos la columna imagenes_id
  CONSTRAINT fk_imagenes_id FOREIGN KEY (imagenes_id) REFERENCES imagenes(id_imagen)
);

-- Tabla Categorias
CREATE TABLE Categorias (
  id_categoria SERIAL PRIMARY KEY,
  nombre VARCHAR(50) NOT NULL
);

-- Tabla Intermedia ProductoCategoria
CREATE TABLE ProductoCategoria (
  id_producto INTEGER NOT NULL,
  id_categoria INTEGER NOT NULL,
  tipo_categoria VARCHAR(50),
  PRIMARY KEY (id_producto, id_categoria),
  FOREIGN KEY (id_producto) REFERENCES Productos(id_producto),
  FOREIGN KEY (id_categoria) REFERENCES Categorias(id_categoria)
);
```

### En caso de haber creado las tablas sin relaciones, se pueden añadir después con el siguiente comando:

```sql

-- Añadir clave foránea

-- Agregar columna imagenes_id y relación de clave externa en la tabla Productos
ALTER TABLE productos
ADD COLUMN imagenes_id INTEGER,
ADD CONSTRAINT fk_imagenes_id FOREIGN KEY (imagenes_id) REFERENCES imagenes(id_imagen);

-- Agregar columna imagenes_id y relación de clave externa en la tabla Servicios
ALTER TABLE servicios
ADD COLUMN imagenes_id INTEGER,
ADD CONSTRAINT fk_imagenes_id FOREIGN KEY (imagenes_id) REFERENCES imagenes(id_imagen);
```

## Descripción de la Instrucción ALTER TABLE

- **ALTER TABLE:** Esta es la instrucción SQL que se utiliza para realizar cambios en la estructura de una tabla existente.

- **productos:** Es el nombre de la tabla en la que queremos hacer cambios.

- **ADD COLUMN imagenes_id INTEGER:** Esto agrega una nueva columna llamada 'imagenes_id' a la tabla 'productos'.

  - La columna es de tipo INTEGER, lo que significa que almacenará valores numéricos enteros.

- **ADD CONSTRAINT fk_imagenes_id FOREIGN KEY (imagenes_id) REFERENCES imagenes(id_imagen):**

  - Esta parte establece una restricción de clave externa (foreign key constraint) en la columna 'imagenes_id' de la tabla 'productos'.

  - La restricción se llama 'fk_imagenes_id'.

  - Esto significa que los valores en la columna 'imagenes_id' de la tabla 'productos' deben hacer referencia a los valores en la columna 'id_imagen' de la tabla 'imagenes'.

  - Esto establece una relación entre las dos tablas, donde 'productos' tiene una referencia a 'imagenes'.

## Elimina la tabla

```sql
-- Eliminar tabla con relaciones:
DROP TABLE nombre_tabla CASCADE;

-- Ejemplos:
DROP TABLE categorias CASCADE;
DROP TABLE imagenes CASCADE;
DROP TABLE productocategoria CASCADE;
DROP TABLE productos CASCADE;
DROP TABLE servicios CASCADE;
```
