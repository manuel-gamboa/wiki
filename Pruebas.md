Las convenciones para las pruebas de software que usaremos serán las siguientes:

* Las pruebas se encuentran dentro de la carpeta test y dentro debemos de crear una estructura de carpetas idénticas a la que se encuentra en el folder src, de la siguiente manera:

```
//codigo
src/app/command/ventas/app.command.ventas.Vender.pas

//prueba
test/app/command/ventas/app.command.ventas.VenderSpec.pas
```

Como pueden ver ambos nombres y rutas son casi idénticos solo cambia que a la prueba se le agrega la palabra spec (specification) al final.

* La clase de pruebas (TextFixture) se llama igual que el archivo
* La clase que se esta probando se puede llamar Subject (sujeto bajo prueba) o llamarla igual que la clase y es siempre el primer campo privado de la prueba.
```pascal
type
  [TestFixture]
  TVenderSpec = class
    private
      subject: TVender; //preferido
      vender: TVender;  //valido
```
* Los casos de prueba (métodos de la clase de pruebas) deben comenzar con Debe, para que se lea de la siguiente manera : "El sujeto debe de".
```pascal
type
  TVenderSpec = class
    private
      subject: TVender; //preferido
      vender: TVender;  //valido
    public
      [Test]
      procedure DebeLanzarErrorSiLaVentaNoTieneArticulos;
  end;
```
* Antes de hacer cualquier push debes de hacer un pull, luego correr las pruebas y si todas pasan entonces hacer tu push, en caso de que alguna prueba no pase debes de arreglarla antes de hacer push.