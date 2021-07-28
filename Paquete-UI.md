### Namespace UI

Este paquete no requiere mucha explicación, contiene las formas y esta dividido por modulo, las formas solo deben conocer al paquete aplicación y al paquete common y deben de ser "tontas", es decir la única lógica permitida en las formas es la que tiene que ver con la visualización, no deben contener ninguna lógica de negocio o de base de datos o de ningún otro tipo.

```
UI.creditos
UI.ventas
UI.facturacion
UI.clientes
```

### Vistas y Modelos

Una practica común y opcional es dividir la lógica en dos partes, **la vista**(forma) solo debe manejar código relacionado con la visualización (estilos, tamaños, animaciones, etc) y el **modelo de la vista** debe manejar el código de interacción con la aplicación, el beneficio es mantener ambas partes mas pequeñas , si la clase hace muchas cosas tener ambas en el código de la forma puede causar que el archivo sea muy grande, otro beneficio es que el modelo se puede testear aunque nosotros no lo haremos, si la clase es pequeña o si el desarrollador lo decide puede mantener ambos juntos, mas adelante intentaremos fabricar o encontrar un mecanismo de "binding" lo que haría esta separación mas útil.

[MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel)