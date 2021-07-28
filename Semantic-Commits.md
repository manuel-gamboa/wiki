Cada desarrollador puede localmente usar cualquier nombre o convención para sus commits, sin embargo al momento de hacer merge en develop o master se debe usar squash merge y usar la siguiente convención para el nombre del commit:

`tipo: descripción (#PRNumber)`

donde el tipo de commit puede ser uno de los siguientes:

* feat: (nuevos features)
* fix: (bug fixes)
* docs: (documentación)
* style: (cambios estéticos, de estilo o formato)
* refactor/perf: (cambios que no son features ni fixes, mejoras al código)
* test: (pruebas de software)
* chore: (cosas que no son código, instaladores, scripts de final builder)
* revert: (revert commits)
* wip: (work in progress, debes de hacerles rebase una vez que terminas tu trabajo para renombrar)

la descripción debe ser un verbo y un pequeño texto:

`fix: correccion bug de facturación`

si se requiere mas explicación se debe usar el cuerpo del commit, por ejemplo si es un bug se puede poner el link a Basecamp, lo mismo para un feature se puede poner el link al Pitch:

```
fix: correccion bug de facturación

closes: http://basecamp.com/hajsshsjsh/bug-facturacion-cliente-internacional
```

el log de develop y master debe verse de la siguiente manera:
![](https://raw.githubusercontent.com/bambucode/resources/main/github1.png)