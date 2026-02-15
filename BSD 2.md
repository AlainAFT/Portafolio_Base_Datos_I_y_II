# Clase1
# Comandos Básicos parte 2

## DCL

## TCL


# Tablas Temporales

caracteristicas 

estructura

`Select into # " Nombre de la tabla temporal "`
`// codigo restante` 


ejemplo

`Select  A.id  into # Autores Actuales`
`from Autores as A`
`inner join Materiales as M`
`on A.id = M.idA`
`where M.tipo = 'A'`
