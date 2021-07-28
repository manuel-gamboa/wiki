Eleventa debe estar preparado para soportar múltiples lenguajes, debido a esto tenemos que manejar todas las cadenas de caracteres de una manera especial, en ninguna parte del código deben aparecer cadenas de caracteres que sean mensajes para el usuario o parte de la interface de usuario, es decir nada que el usuario final pueda leer, para lograr esto usaremos archivos con variables, existe uno por cada modulo y debe cargarse en donde se necesite de la siguiente manera:

```pascal
uses
  i18n_Credito;  //archivo de mensajes para modulo Creditos

procedure TFormaCredito.BtnAplicarCargoClick;
begin
  AplicarCargo(edtCargo.Text);
  MostrarAviso(i18n.CargoAplicado); //contiene el mensaje en el lenguaje cargado
end;
```

Si debes agregar un nuevo mensaje debes ir a la unidad del modulo correspondiente y agregarlo como una variable estática, después agregar el mensaje en los archivos de lenguaje con el mismo nombre que usaste en la variable estática:

```pascal
//unit i18n_Credito
type
  i18n = class
    public
      class var CargoAplicado: string;
      class var NuevoMensaje: string;
  end;

//JSON lang/mx/Credito.json
{
  CargoAplicado: "El Cargo fue aplicado",
  NuevoMensaje: "Este es un nuevo mensaje"
}

//JSON lang/es/Credito.json
{
  CargoAplicado: "Os informamos que el cargo fue aplicado",
  NuevoMensaje: "Por favor verifica vuestro internet en el aparcadero, ostias"
}
```

En el archivo de configuración que veremos más adelante se carga el json con el lenguaje seleccionado por el usuario (por default mx).