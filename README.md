/*En primer lugar seleccionamos la base de datos con la que vamos a trabajar*/

use bd1_practica1
SHOW TABLES; //Desplego todos

/* Podemos desplegar las columnas..*/
DESCRIBE libro;

/*Crear una base de datos*/
CREATE database bd1_practica2

/*crear tablas*/
CREATE TABLE revista(
    isbn INT(11) NOT NULL PRIMARY KEY;
    titulo VARCHAR(90) NULL;
    id_editorial int null;
    precio DECIMAL (8,2);
)
SHOW TABLES;
DESCRIBE Revista;
/*Clave Primaria compuesta*/
CREATE TABLE a(
    da1 INT NOT NULL;
    da2 INT NOT NULL;
    Descripcion varchar(90);
    PRIMARY KEY (da1, da2);


)
DESCRIBE a;

/*Modificar una tabla-
ALTER*/
/*modificación de la columna*/
ALTER TABLE a 
MODIFY COLUMN Descripcion VARCHAR (1000);

/*podemos decirle que no sea nulo */
ALTER TABLE a 
MODIFY COLUMN Descripcion VARCHAR (1000) NOT NULL;

/*Eliminar una columna:*/
ALTER TABLE a DROP COLUMN Descripcion;

/*Agregar una columna*/
ALTER TABLE a ADD COLUMN Descripcion varchar(100) NULL;

/*Eliminar una tabla*/
DROP table a;

/*Crear con auto-incremente*/
CREATE TABLE a (
    da1 INT NOT NULL auto_increment;
    da2 INT NOT NULL;
    Descripcion varchar(90);
    PRIMARY KEY (da1, da2);
)
/*VISTAS*/
CREATE VIEW libro_completo AS
SELECT l.isbn, l.titulo, l.precio, e.nombre, e.ciudad
FROM libro l INNER JOIN editorial e
ON l.id_editorial=e.id_editorial;
 
SELECT * FROM libro_completo;
SELECT l.ciudad, l.precio from libro_completo l WHERE l.ciudad= "Buenos Aires";
SELECT l.ciudad, sum(l.precio) from libro_completo l
GROUP BY l.ciudad;

/*Modificar la vista*/
ALTER VIEW libro_completo as 
SELECT l.titulo, l.precio, e.nombre, e.ciudad
FROM libro l INNER JOIN editorial e
ON l.id_editorial=e.id_editorial;


/*Si se modifica la tabla se modifica los datos de la vista*/


/*Y se cambio la vista..*/


/*Eliminar la vista */
DROP VIEW libro_completo;

/*INSERT*/
INSERT INTO revista (isrn, titulo, id_editorial, precio)
VALUES (1234, 'revista 1', 1, 1000);

Estamos insertando 1 dato, explícitamente, 
escribimos los datos que van a haber en cada fila que se inserta.


/*Que pasa si no indico las columnas, si agrego una columna, 
que no debería haber ningún conveniente para consultas Y 
hago un insert me va a decir que hay un error que hay distinta cantidad de columnas
entre lo que estoy insertando y el esquema de la tabla.*/
ALTER TABLE revista
ADD COLUMN ciudad VARCHAR(50);

INSERT INTO revista (isrn, titulo, id_editorial, precio)
VALUES (5677, 'revista 3', 1, 10);

/*Puedo insertar más de una fila*/

INSERT INTO revista (isrn, titulo, id_editorial, precio)
VALUES (7899, 'revista 4', 2, 20),
(3444, 'revista 5', 3, 25);

/*El comando INSERT también se puede usar para insertar datos 
en una tabla de otra tabla. 

INSERT INTO table_1 SELECT * FROM table_2;*/


INSERT INTO revista (isrn, titulo, id_editorial, precio)    //Inserto en revista haciendo un select desde la tabla libro de la tabla 1 a la tabla 2
SELECT isbn, titulo, id_editorial, precio                   //Columnas que quiero insertar
FROM libro;

/*También le podría poner una restricción*/
INSERT INTO revista (isrn, titulo, id_editorial, precio)
SELECT isbn, titulo, id_editorial, precio
FROM libro where tipo='revista'

/*UPDATE:
La sentencia UPDATE se utiliza para modificar valores en una tabla.
La sintaxis de SQL UPDATE es:
UPDATE nombre_tabla
SET columna1 = valor1, columna2 = valor2
WHERE columna3 = valor3*/
ALTER TABLE revista ADD COLUMN VARCHAR(50)
UPDATE revista
SET ciudad='Buenos Aires' WHERE isrn=1234;

UPDATE revista
SET ciudad='Buenos Aires'   //Sin el where a todos se les coloca "BUENOS AIRES" en su ciudad

UPDATE revista
SET ciudad=CONCAT(titulo, ' ','Buenos Aires') WHERE isrn=1234;  //CONCAT concatena Strings 
                                                                //Concatena el titulo de la revista + un espacio + ciudad de buenos aires
UPDATE revista
SET precio=precio*2;


UPDATE revista r SET ciudad = (     //Siempre devuelve el mismo dato en este ejemplo ciudad
SELECT e.ciudad
from editorial e
WHERE e.id_editorial=r.id_editorial);   //pone la ciudad que le corresponde 

/*DELETE
Para eliminar una o más filas de una tabla por completo, usamos DELETE
la siguiente declaración eliminará todas las filas de revista:*/
DELETE FROM revista

/*Eliminar algunas filas con un ejemplo de condición*/
Delete from revista where isrn<3000;

CREATE table revista2 (
id INT NOT NULL auto_increment PRIMARY key,
isrn INT NOT NULL ,
titulo VARCHAR(90) null,
id_editorial INT NULL,
precio DECIMAL(8,2) );

INSERT INTO revista2 (isrn, titulo, id_editorial, precio)
VALUES (7899, 'revista 4', 2, 20);
SELECT * FROM revista2;

DELETE from revista2 WHERE id=1;

INSERT INTO revista2 (isrn, titulo, id_editorial, precio)
VALUES (7899, 'revista 4', 2, 20):

/*Sigue con la numeración de los id.*/

Truncate table revista2;

INSERT INTO revista2 (isrn, titulo, id_editorial, precio)
VALUES (7899, 'revista 4', 2, 20)

/*La tabla reinicia el ID*/


