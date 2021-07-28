### Deben ser pequeños

Si una función es demasiado larga se complica su análisis, si contiene mas de 30 lineas de código es muy probable que se pueda dividir en múltiples funciones.

### Deben hacer una sola cosa 

Si encontramos una función que hace varias cosas debemos extraer la funcionalidad en múltiples funciones, un code smell muy importante es cuando encontramos comentarios que indican lo que hace una parte del código, si encontramos este tipo de comentarios lo mas probable es que tengamos que extraer la funcionalidad.

```pascal
//Mal
function TMyClass.GenerarComandoGraphQL(
  const aEntidadOriginal: TEntidadDTO;
  const aEntidadModificada): GraphQLCommand;
begin
  //Obtener las diferencias entre ambas entidades
  ...
  ...

  //Validar si no existe una entidad anidada en las diferencias
  ...
  ...

  //Generar el comando
  ...
  ...
end; 

//Bien
function TMyClass.GenerarComandoGraphQL(
  const aEntidadOriginal: TEntidadDTO;
  const aEntidadModificada): GraphQLCommand;
var
  delta: TDelta;
begin
  delta := ObtenerDelta(aEntidadOriginal, aEntidadModificada);

  ValidarEntidadesAnidadas(delta);

  Result := GenerarComando(delta); 
end;
```

### Debe usar pocos argumentos 

El numero de argumentos máximo debe ser 3, si se necesitan mas debemos crear un objeto que contenga todos los datos, esta regla y todas las demás pueden romperse, si es totalmente necesario usar mas de 3 argumentos sin usar una clase se puede hacer, pero deberán ser muy pocos los casos que requieran romper las convenciones.

```pascal
//Mal
procedure TMyClass.GuadarCliente(
  const aNombre: String;
  const aDomicilio: String;
  const aEdad: Integer;
  const aNacionalidad: String);

//Bien
procedure TMyClass.GuardarCliente(const aCliente: TClienteDTO);
```
