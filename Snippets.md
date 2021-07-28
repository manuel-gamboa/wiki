Delphi soporta el uso de snippets/[Live Templates](http://docwiki.embarcadero.com/RADStudio/Seattle/en/Live_Templates), además de los que Delphi ya posee, tenemos una carpeta compartida de snipepts específicos para eleventa. La carpeta donde se encuentran en en `Utilidades\Snippets\` de la carpeta raíz del proyecto.

Esta carpeta contiene plantillas de código que compartimos al usar eleventa usando la función de "Live Templates" de Delphi.

Para una introducción sobre los Live Templates puedes ver el siguiente video:

[![Delphi Live Templates](https://img.youtube.com/vi/xfA6SHlzPF0/0.jpg)](https://www.youtube.com/watch?v=xfA6SHlzPF0)

### Configuración Inicial
Para configurar tu máquina e instalación de Delphi para que funcione, deberás hacer lo siguiente:

1. Abrir una consola de linea de comandos en modo Administrador.
2. Ejecutar el siguiente código una vez: 

```rmdir "C:\Users\soporte\Documents\Embarcadero\Studio\code_templates\Delphi" & mklink /D "C:\Users\soporte\Documents\Embarcadero\Studio\code_templates\Delphi" C:\Delphi\eleventa\Utilidades\Snippets```

## ¿Cómo usarlos?
Con solo teclear la abreviación del snippet y dar enter o Tab será suficiente. Si presionas `CTRL + J` podrás ver la lista completa de Live Templates disponibles.

## ¿Cómo crear un nuevo Live Template?
Debes comenzar creando un archivo XML en dicha carpeta y luego [seguir las instrucciones para crear un Live Template](http://docwiki.embarcadero.com/RADStudio/Seattle/en/Creating_Live_Templates).