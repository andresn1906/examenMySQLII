## 1.- Comandos DDL:

```sql
DROP DATABASE examenCasn;
CREATE DATABASE examenCasn;
USE examenCasn;

CREATE TABLE IF NOT EXISTS pais (
    id_pais SMALLINT UNSIGNED,
    nombre VARCHAR(50),
    ultima_actualizacion TIMESTAMP,
    PRIMARY KEY(id_pais)
);

CREATE TABLE IF NOT EXISTS ciudad (
    id_ciudad SMALLINT UNSIGNED,
    nombre VARCHAR(50),
    id_pais SMALLINT UNSIGNED,
    CONSTRAINT FK_id_pais_ci FOREIGN KEY (id_pais) REFERENCES pais(id_pais),
    PRIMARY KEY(id_ciudad)
);

CREATE TABLE IF NOT EXISTS direccion (
    id_direccion SMALLINT UNSIGNED,
    direccion VARCHAR(50),
    direccion2 VARCHAR(50),
    distrito VARCHAR(50),
    id_ciudad SMALLINT UNSIGNED,
    CONSTRAINT FK_id_ciudad_dir FOREIGN KEY (id_ciudad) REFERENCES ciudad(id_ciudad),
    codigo_postal VARCHAR(10),
    telefono VARCHAR(20),
    ultima_actualizacion TIMESTAMP,
    PRIMARY KEY(id_direccion)
);

CREATE TABLE IF NOT EXISTS idioma (
    id_idioma TINYINT UNSIGNED,
    nombre CHAR(20),
    ultima_actualizacion TIMESTAMP,
    PRIMARY KEY(id_idioma)
);

CREATE TABLE IF NOT EXISTS actor (
    id_actor SMALLINT UNSIGNED,
    nombre VARCHAR(45),
    apellidos VARCHAR(45),
    ultima_actualizacion TIMESTAMP,
    PRIMARY KEY(id_actor)
);

CREATE TABLE IF NOT EXISTS categoria (
    id_categoria TINYINT UNSIGNED,
    nombre VARCHAR(25),
    ultima_actualizacion TIMESTAMP,
    PRIMARY KEY(id_categoria)
);

CREATE TABLE IF NOT EXISTS film_text (
    film_id SMALLINT,
    title VARCHAR(255),
    description TEXT,
    PRIMARY KEY(film_id)
);

CREATE TABLE IF NOT EXISTS pelicula (
    id_pelicula SMALLINT UNSIGNED,
    titulo VARCHAR(255),
    descripcion TEXT,
    anyo_lanzamiento YEAR,
    id_idioma TINYINT UNSIGNED,
    CONSTRAINT FK_id_idioma_pel FOREIGN KEY (id_idioma) REFERENCES idioma(id_idioma),
    id_idioma_original TINYINT UNSIGNED,
    CONSTRAINT FK_id_idioma_original_pel FOREIGN KEY (id_idioma_original) REFERENCES idioma(id_idioma),
    duracion_alquiler TINYINT UNSIGNED,
    rental_rate DECIMAL(4,2),
    duracion SMALLINT UNSIGNED,
    replacement_cost DECIMAL(5,2),
    clasificacion ENUM('G', 'PG', 'PG-13', 'R', 'NC-17'),
    carecteristicas_especiales SET('Trailers', 'Commentaries', 'Deleted Scenes', 'Behind the Scenes'),
    ultima_actualizacion TIMESTAMP,
    PRIMARY KEY(id_pelicula)
);

CREATE TABLE IF NOT EXISTS pelicula_actor (
    id_actor SMALLINT UNSIGNED,
    CONSTRAINT FK_id_actor_pa FOREIGN KEY (id_actor) REFERENCES actor(id_actor),
    id_pelicula SMALLINT UNSIGNED,
    CONSTRAINT FK_id_pelicula_pa FOREIGN KEY (id_pelicula) REFERENCES pelicula(id_pelicula),
    PRIMARY KEY(id_actor, id_pelicula),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS pelicula_categoria (
    id_categoria TINYINT UNSIGNED,
    id_pelicula SMALLINT UNSIGNED,
    PRIMARY KEY(id_pelicula, id_categoria),
    CONSTRAINT FK_id_categoria2 FOREIGN KEY (id_categoria) REFERENCES categoria(id_categoria),
    CONSTRAINT FK_id_pelicula2 FOREIGN KEY (id_pelicula) REFERENCES pelicula(id_pelicula),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS almacen (
    id_almacen TINYINT UNSIGNED,
    id_direccion SMALLINT UNSIGNED,
    CONSTRAINT FK_id_direccion_al FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion),
    PRIMARY KEY(id_almacen),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS empleado (
    id_empleado TINYINT UNSIGNED,
    nombre VARCHAR(45),
    apellidos VARCHAR(45),
    id_direccion SMALLINT UNSIGNED,
    CONSTRAINT FK_id_direccion_em FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion),
    imagen BLOB,
    email VARCHAR(50),
    id_almacen TINYINT UNSIGNED,
    CONSTRAINT FK_id_almacen_em FOREIGN KEY (id_almacen) REFERENCES almacen(id_almacen),
    activo TINYINT(1),
    username VARCHAR(16),
    password VARCHAR(40),
    PRIMARY KEY(id_empleado),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS inventario (
    id_inventario MEDIUMINT UNSIGNED,
    id_pelicula SMALLINT UNSIGNED,
    CONSTRAINT FK_id_pelicula_inv FOREIGN KEY (id_pelicula) REFERENCES pelicula(id_pelicula),
    id_almacen TINYINT UNSIGNED,
    CONSTRAINT FK_id_almacen_inv FOREIGN KEY (id_almacen) REFERENCES almacen(id_almacen),
    PRIMARY KEY(id_inventario),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS cliente (
    id_cliente SMALLINT UNSIGNED,
    id_almacen TINYINT UNSIGNED,
    CONSTRAINT FK_id_almacen_cl FOREIGN KEY (id_almacen) REFERENCES almacen(id_almacen),
    nombre VARCHAR(45),
    apellidos VARCHAR(45),
    email VARCHAR(50),
    id_direccion SMALLINT UNSIGNED,
    CONSTRAINT FK_id_direccion_cl FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion),
    activo TINYINT(1),
    PRIMARY KEY(id_cliente),
    fecha_creacion DATETIME,
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS alquiler (
    id_alquiler INT UNSIGNED,
    fecha_alquiler DATETIME,
    id_inventario MEDIUMINT UNSIGNED,
    CONSTRAINT FK_id_inventario_al FOREIGN KEY (id_inventario) REFERENCES inventario(id_inventario),
    id_cliente SMALLINT UNSIGNED,
    CONSTRAINT FK_id_cliente_al FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente),
    fecha_devolucion DATETIME,
    id_empleado TINYINT UNSIGNED,
    CONSTRAINT FK_id_empleado_al FOREIGN KEY (id_empleado) REFERENCES empleado(id_empleado),
    PRIMARY KEY(id_alquiler),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS pago (
    id_pago SMALLINT UNSIGNED,
    id_cliente SMALLINT UNSIGNED,
    CONSTRAINT FK_id_cliente_pag FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente),
    id_empleado TINYINT UNSIGNED,
    CONSTRAINT FK_id_empleado_pag FOREIGN KEY (id_empleado) REFERENCES empleado(id_empleado),
    id_alquiler INT UNSIGNED,
    CONSTRAINT FK_id_alquiler_pag FOREIGN KEY (id_alquiler) REFERENCES alquiler(id_alquiler),
    total DECIMAL(5,2),
    PRIMARY KEY(id_pago),
    fecha_pago DATETIME,
    ultima_actualizacion TIMESTAMP
);
```
