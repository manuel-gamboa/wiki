### Debemos usar Excepciones personalizadas 

Debemos crear nuestras propias excepciones que describan el problema y posiblemente transporten datos específicos.

```pascal
//Bien en algunas ocasiones
except 
  On E: Exception do

//Mejor
except
  On E: EUserValidationException do
```

### Debemos lanzar excepción cuando no se cumplan pre-condiciones

Si las pre-condiciones de un método no se cumplen debemos lanzar una excepción, a esto se le conoce como Guards, si no se usan guardas el código podría complicarse ya que puede utilizar múltiples bifurcaciones.

```pascal
//Mal
function AplicarCargoACredito(const aVenta: TVenta; const aClienteId: Integer): Boolean;
var
  credito: TCredito;
begin
  Result := True;
  credito := fRepositorio.ObtenerCredito(aClienteId);

  if Assigned(credito) then
  begin
    if credito.CreditoDisponible > 0 then
    begin
      credito.AplicarCargo(aVenta.Monto);
    end
    else
      Result := False;
  end
  else
    Result := False;
end;  

//Bien
function AplicarCargoACredito(const aVenta: TVenta; const aClienteId: Integer): Boolean;
var
  credito: TCredito;
begin
  credito := fRepositorio.ObtenerCredito(aClienteId);

  if not Assigned(credito) then
    raise EValidacionException.Create('El cliente no tiene un crédito activo');
  
  if credito.CreditoDisponible = 0 then
    raise EValidacionException.Create('El cliente no tiene crédito disponible');

  //Si paso todas las guardas ahora si comenzamos con la lógica.
  ....
  ....
end;
```