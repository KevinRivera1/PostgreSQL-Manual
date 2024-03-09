# Manual de PostgreSQL

Este manual proporciona una guía paso a paso para aprender PostgreSQL, un sistema de gestión de bases de datos relacional de código abierto y potente.

## Contenido

1. [Introducción](#introducción)
2. [Instalación](#instalación)
3. [Conceptos Básicos](#conceptos-básicos)
4. [Operaciones Básicas](#operaciones-básicas)
5. [Modificación de Tablas](#modificación-de-tablas)
6. [Eliminación de Tablas](#eliminación-de-tablas)

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

## Relacion de tablas uno auno

```sql
-- Crear tabla
-- Crear la tabla persona
CREATE TABLE persona (
  id_persona SERIAL PRIMARY KEY,
  nombre VARCHAR(50) NOT NULL,
  apellido VARCHAR(50) NOT NULL,
  edad INTEGER
);

-- Crear la tabla direccion
CREATE TABLE direccion (
  id_direccion SERIAL PRIMARY KEY,
  calle VARCHAR(100) NOT NULL,
  ciudad VARCHAR(50) NOT NULL,
  codigo_postal VARCHAR(10) NOT NULL,
  id_persona INTEGER UNIQUE,  -- Se añade la restricción UNIQUE para asegurar que cada dirección esté asociada a una sola persona
  FOREIGN KEY (id_persona) REFERENCES persona(id_persona) ON DELETE CASCADE  -- Se establece la clave foránea con opción de eliminación en cascada
);
```

### Explicación detallada de la Relación Uno a Uno

En la definición de la tabla `direccion`, hemos establecido una restricción de clave foránea (**FOREIGN KEY**), que es una regla que asegura la consistencia de los datos entre dos tablas relacionadas en una base de datos relacional.

- **Clave Foránea (FOREIGN KEY):** Esta restricción garantiza la integridad referencial entre dos tablas. En este caso, la clave foránea se ha aplicado a la columna `id_persona` en la tabla `direccion`.

- **Referencia a la tabla `persona`:** La clave foránea en la columna `id_persona` de la tabla `direccion` está referenciando la columna `id_persona` en la tabla `persona`. Esto significa que los valores en la columna `id_persona` de la tabla `direccion` deben coincidir con los valores en la columna `id_persona` de la tabla `persona`.

- **ON DELETE CASCADE:** Esto es una especificación adicional que hemos añadido a la restricción de clave foránea. Cuando se establece en `ON DELETE CASCADE`, indica que si se elimina una fila en la tabla `persona`, todas las filas relacionadas en la tabla `direccion` que contienen el mismo valor en la columna `id_persona` también se eliminarán automáticamente. Esta acción es útil para mantener la integridad referencial y evitar registros huérfanos en la base de datos.

## Colaboración y Contribuciones

¡Tu contribución es bienvenida para mejorar este manual! Si tienes sugerencias para agregar contenido adicional, corregir errores o mejorar la claridad de la información, ¡no dudes en participar!

### Cómo Contribuir

1. Haz un clone de este repositorio y clona tu copia localmente:

```bash
git clone https://github.com/TuUsuario/NombreDelRepositorio.git
```

2. Crea una nueva rama para tu contribución:

```bash
git checkout -b nueva-funcionalidad
```

3. Crea un Pull Request (PR) desde tu rama en GitHub.
4. Describa detalladamente tus cambios en el PR y espera comentarios o revisiones.

### Informar Problemas

- Esta sección proporciona instrucciones claras sobre cómo contribuir al proyecto a través de `pull requests` en GitHub, así como cómo informar problemas o sugerencias a través de `issues`.
