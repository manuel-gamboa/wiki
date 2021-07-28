### Namespace configuration.

Es una carpeta top level al igual que src y test.

Al igual que common, config contiene clases que se pueden utilizar en cualquier lugar del software, la diferencia esta en el tipo de información que contienen, las clases de config representan cosas que pueden cambiar de un momento a otro según la configuración seleccionada, por ejemplo los datos de conexión a la base de datos o conexión a un webservice o el registro de implementaciones del Contenedor IoC, en cambio common tiene funcionalidad usada en todos lados pero que no cambia comúnmente como el logeo de errores, excepciones, datos del usuario logeado en el sistema actualmente, etc. 

Common se considera código, por eso esta dentro de src, config no se considera código ya que generalmente solo contiene datos (variables o constantes) y su única funcionalidad es cargar datos de archivos o variables externas al programa. 

Es una clase estatica llamada Config y su namespace es Configuración.
```pascal
uses
  Configuracion;

procedure Connect;
begin
  connection.server := Config.db.server;
  connection.username := Config.db.user;
  connection.password := Config.db.pass;
  ...
end;
```