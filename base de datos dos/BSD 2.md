# Clase1

13 de febrero de 2026
# Comandos Básicos parte 2

## DCL

## TCL


# Tablas Temporales

**características** 

*Solo existen en su pagina en la que fue creada y no en las demas*

**eliminar :**  aunque se elimina por si solo y me refiero cuando cierres la sesión de SQL o tu gestor de base de datos se eliminara automáticamente .
pero puedes usar 

`Delete if exist`

ejemplo : 

`IF OBJECT_ID('tempdb..#NombreTabla') IS NOT NULL`
    `DROP TABLE #NombreTabla;`   

estructura :

`Select into # " Nombre de la tabla temporal "`
`// codigo restante` 


ejemplo

`Select  A.id  into # Autores Actuales`
`from Autores as A`
`inner join Materiales as M`
`on A.id = M.idA`
`where M.tipo = 'A'`


# clase 2 

16 de febrero de 2026


