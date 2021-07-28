El agregar un permiso es una tarea fácil pero de múltiples pasos. Lo que hay que hacer es lo siguiente:

1. Editar el archivo `uPermisos.pas` y agregar el nuevo permiso **al final del archivo** siguiendo la misma convención. Ejemplo: `pcMiNuevoPermiso`.
2. Cerrar Delphi.
3. Borrar el cache del componente y DCUs: `uPermisos.dcu`, `InterfacePermisos.bpl`, `TBambuCheckBoxPermisos.bpl` usando la app "Everything" para eliminar todas las instancias compiladas de tu computadora.
4. Editar el proyecto: `InterfacePermisos.dpk`, recompilar. Copiar el archivo `InterfasePermisos.bpl` recién generado a la ruta de binarios de Delphi: `C:\Users\Public\Documents\Embarcadero\Studio\17.0\Bpl`.
5. Editar el proyecto del componente que lo lee: `TBambuCheckBoxPermisos.dproj`. Abrir `TBambuCheckBoxPermisos.dproj`, en el IDE dar click en la pestaña View -> Project Manager, se mostrará el proyecto `TBambuCheckBoxPermisos`, click derecho -> Build, luego click derecho -> Install.
si **NO APARECEN** las opciones, en la barra de proyectos (Usualmente a la derecha) dar click derecho en `TBambuCheckBoxPermisos.bpl` ->build, luego click derecho -> Install. 

**Importante:** Para evitar que un compañero tuyo quite un permiso sin querer del DFM notifica a todo el equipo para que realice este proceso una vez que tu código esté en el branch de trabajo (master, develop, hotfix, etc)