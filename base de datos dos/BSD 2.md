#sqlserver  #ISW #SIS-221  #semetre_3 #_2026  #base_de_datos_II
# Clase 1

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

# Tabla Temporales Globales


**Definición:**

Son lo contrario de las locales por el nombre nos indica de que este tipo tablas pueden ser vista por todas las sesiones que tengamos anidadas , y al igual que las ==tablas temporales== se eliminaran cuando la sesión se desconecta y ya no hay mas referencias a la tabla.


Siempre puede tratar de usar el comando “DROP” para limpiarlo manualmente. Lo cual es algo que le recomendaría.

![Diagrama](https://s33046.pcdn.co/wp-content/uploads/2017/02/word-image-82.png)

**declaración**:

Para tratar de crear una tabla temporal global de SQL, simplemente use los dos símbolos de libra delante del nombre de la tabla.

**Ejemplo :**
    `## Global_Table_Name.`

# variables de tabla

las variables se declaran utilizando la instrucción de  ==DECLARE== 

Muchos creen que las variables de tabla existen solo en la memoria, pero eso simplemente no es cierto. Ya que residen en la base de datos tempdb de forma muy similar a las tablas temporales de SQL Server locales.

En lo que respecta al rendimiento, las variables de tabla son útiles con pequeñas cantidades de datos (como solo unas pocas filas). De lo contrario, una tabla temporal de SQL Server es útil al examinar grandes cantidades de datos. Ahora, para la mayoría de los scripts, lo más probable es que vea el uso de una tabla temporal de SQL Server en lugar de una variable de tabla. Esto no quiere decir que uno sea más útil que el otro, solo tiene que elegir la herramienta adecuada para el trabajo.

**EJEMPLO:**


```
DECLARE @TotalProduct AS TABLE

(ProductID INT NOT NULL PRIMARY KEY,

Quantity INT NOT NULL)

INSERT INTO @TotalProduct

         ( [ProductID], [Quantity] )

SELECT

  A.[ProductID],

  [Quantity] = SUM(B.Quantity)

FROM dbo.Product AS A

INNER JOIN dbo.SalesDetails AS B ON A.ProducitID = B.ProductID
```
# clase 2 

16 de febrero de 2026



[[Tareas#Practica 1 - Plan de ejecución]]


# Enlaces

## clase 1

[Cuándo utilizar las tablas temporales de SQL frente a las variables de tabla September 16, 2019 by Aamir Syed](https://www.sqlshack.com/es/cuando-utilizar-las-tablas-temporales-de-sql-frente-a-las-variables-de-tabla/)
