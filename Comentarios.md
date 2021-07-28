### Deben explicar por que 

Un comentario que explica que hace el código se considera un code smell , si acaso se usan comentarios debería explicar el por que.

```pascal
//Mal, los comentarios están de mas ya que solo están repitiendo lo que el código ya expresa.

//Obtenemos el cliente
Cliente := fRepositorio.ObtenerCliente(ClienteId);

//Validamos que tenga crédito
if Assigned(fRepositorioCreditos.ObtenerCredito(ClienteId)) then
  ...
end;

//Aplicamos el cargo de la venta
Credito.AplicarCargo(aVenta.Total);

//Bien
{
  En este caso usamos el método de búsqueda RadixSort por que mediante
  benchmarks determinamos que tiene un mejor performance
  que QuickSort para este caso en especifico. 
}
```

> In general, the best advice we can give is to keep your code free of comments. 

### Código Muerto

Nunca debemos dejar código comentado, mientras estas trabajando con el código puedes hacerlo pero debemos removerlo antes de hacer push.

```pascal
//Debemos evitar hacer push de comentarios de este tipo:

//TCliente.Cargo(aMonto);
TCliente.AplicarCargo(aMonto);


//  for I := 0 to 1000 do
//  begin
//    SomeLogic(Transacciones[I]);
//    ...
//  end;
   
Transacciones.ForEach(
  //someLogic;
);
```