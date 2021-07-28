Este paquete contiene las clases que se usan de manera global, es decir que todos o muchos paquetes utilizan, inicialmente solo contiene los paquetes internos Error y Tipo pero aquí deben de ir las clases que manejan el logeo y la autenticación.

1. **Error** Contiene las excepciones.

```pascal
type
  EDatabaseException = class(Exception);
```

2. **Tipo** Contiene los Sets que se usan como tipos.

```pascal
type
  TFormaDePago = (fpEfectivo, fpTarjeta, fpCredito);
```