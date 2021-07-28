### Alineación de variables 

No debemos usar espacios para simular columnas en las declaraciones de variables o asignaciones, hacer esto es algo que se ve bien pero es una practica poco común en los lenguajes de alto nivel y muy usada en los lenguajes de bajo nivel (ensamblador, C).

```pascal
//Mal
begin
  credito                 := TCredito.Create;
  cliente                 := TCliente.Create; 
  transaccion             := TTransaccion.Create;
  controladorDevoluciones := TControladorDevoluciones.Create;

//Bien
begin
  credito := TCredito.Create;
  cliente := TCliente.Create;
  transaccion := TTransaccion.Create;
  controladorDevoluciones := TControladorDevoluciones.Create;
```

### Sangría 

Usaremos 2 espacios en blanco como sangría y no debemos dejar de indentar aunque sea muy corto el código.

```pascal
//Mal
if EstaActivo then estado := stActivo
else estado := stInactivo;

//Bien
if EstaActivo then
  estado := stActivo
else
  estado := stInactivo;
```

### Espacio Vertical

Debemos separar con un renglón en blanco cuando sea necesario, agrupando "bloques" de código.

```pascal
//Mal
Result := TCommand.Create;
contador := 0;
while contador < 100 do
begin
  SomeLogic();
end;

//Bien
Result := TCommand.Create;

contador := 0;

while contador < 100 do
begin
  SomeLogic();
end;
```

### Omitir begin y end 

Cuando la lógica conste de una sola linea de código debemos omitir el bloque.

```pascal
//Mal
if Assigned(credito) then
begin
  credito.AplicarAbono(aMonto);
end;

//Bien
if Assigned(credito) then
  credito.AplicarAbono(aMonto);
```

### Códigos multi linea 

Cuando el código es demasiado largo y es necesario usar múltiples renglones para una sola operación (statement) se propone el siguiente formato.

```pascal
//Mal
begin
  Creditos := db.ExecuteQuery(Format('Select * from creditos where clienteId = %d', [aClienteId]));

  Creditos := db.ExecuteQuery(Format(
                                     'Select id,nombre,direccion,telefono,saldo ' + 
                                     'from creditos where clienteId = %d',
                                     [aClienteId]));
//Bien
//Java Like or JS Like
  Creditos := db.ExecuteQuery(
    Format(
      'Select id,saldo,estaActivo,nivel from creditos ' + 
      'where clienteId = %d and estaActivo = ''%s'' and nivel = ''%s''',
      [
        aRequest.clienteId,
        db.True,
        aRequest.nivelDeCredito
      ]
    )
  );

  Creditos :=
    db.ExecuteQuery(
      Format( 
        'Select id,nombre,direccion,telefono,saldo ' + 
        'from creditos where clienteId = %d',
        [
          aRequest.clienteId,
          db.True,
          aRequest.nivelDeCredito
        ] 
      )   
    );

//C# Like
  Creditos :=
    db.ExecuteQuery
    (
      Format
      ( 
        'Select id,nombre,direccion,telefono,saldo ' + 
        'from creditos where clienteId = %d',
        [
          aRequest.clienteId,
          db.True,
          aRequest.nivelDeCredito
        ] 
      )   
    );
```