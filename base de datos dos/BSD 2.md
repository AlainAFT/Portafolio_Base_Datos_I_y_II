#sqlserver 
# Clase1

13 de febrero de 2026
# Comandos Básicos parte 2

## DCL

## TCL


# Tablas Temporales Locales

**características** 

*Solo existen en su pagina en la que fue creada y no en las demas*

*eliminar :*  aunque se elimina por si solo y me refiero cuando cierres la sesión de SQL o tu gestor de base de datos se eliminara automáticamente .
pero puedes usar 

`Delete if exist`

*ejemplo :* 

`IF OBJECT_ID('tempdb..#NombreTabla') IS NOT NULL`
    `DROP TABLE #NombreTabla;`   

*estructura :*

`Select into # " Nombre de la tabla temporal "`
`// codigo restante` 


*ejemplo :*

`Select  A.id  into # Autores Actuales`
`from Autores as A`
`inner join Materiales as M`
`on A.id = M.idA`
`where M.tipo = 'A'`

**Implementaciones :**

Nos sirve para poder alivianar las consultas que hacemos en SQL , normalmente sirve como para guardar resultados de una unión y guardarla para ocuparla para que no se repite pero obviamente si esta dentro de un ciclo de algún tipo. 

Esto no solo nos ahorra el costoso procesamiento de consultas, sino que incluso puede ayudar a hacer que su código se vea un poco más limpio.

==Sin embargo, hay un punto que quiero resaltar. Si la sesión en la que estamos trabajando tiene sesiones anidadas posteriormente, las tablas temporales de SQL Server serán visibles en las sesiones inferiores de la jerarquía, pero no arriba en la jerarquía. Permítame visualizar esto.==

**Diagrama :**

En este diagrama rápido, se va a crear una tabla temporal de SQL en la sesión 2. Las sesiones siguientes (sesiones 3 y 4) pueden ver la tabla temporal de SQL Server. Pero la sesión 1, que está por encima de la sesión 2, no podrá ver la tabla temporal de SQL Server.



![Diagrama](https://www.sqlshack.com/wp-content/uploads/2017/02/word-image-81.png)



La tabla temporal de SQL se descarta o destruye una vez que la sesión se desconecta. Muchas veces podrá ver que los desarrolladores usan el comando “DROP # Table_Name” al final de su declaración solo para poder limpiar la tabla. Pero depende totalmente de usted y de lo que está tratando de lograr.

También tiene que tener en cuenta que, en caso de conflicto de nombres (recuerde que las tablas temporales de SQL Server se crean en tempdb), el servidor SQL agregará un sufijo al final del nombre de la tabla para que sea único dentro de la base de datos tempdb. Pero este proceso es transparente para el desarrollador/usuario. Puede utilizar el mismo nombre que declaró ya que está limitado a esa sesión.

# Tabla Temporales 


# clase 2 

16 de febrero de 2026



