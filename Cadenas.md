### No debemos usar cadenas de caracteres en el código 

esto debido a que eleventa soporta(soportará) múltiples lenguajes, en lugar de usar una cadena debemos usar una variable que represente a la cadena, esta variable se asigna cuando eleventa se ejecuta dependiendo del lenguaje seleccionado por el usuario, para poder usar las variables se usa la unidad i18n_modulo la cual contiene todas las cadenas del modulo:

```pascal
uses
  i18n_Creditos;

procedure AplicarCargo;
begin
  if AplicarCargo.Ejecutar(edtMonto.Text).Success then
    MostrarAviso(i18n.CargoAplicado);  
end;
```