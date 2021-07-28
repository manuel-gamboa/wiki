### No debemos usar valores "mágicos" 

Si no esta perfectamente claro que significa un valor debemos usar una constante para clarificar su intención.

```pascal
//Mal
total := aTransaccion.Monto * 0.15;

//Bien
const
  COMISION_DEFAULT: Currency := 0.15;
begin
  total := aTransaccion.Monto * COMISION_DEFAULT;
```