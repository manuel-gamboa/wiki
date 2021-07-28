### Debe tener una sola responsabilidad

si encontramos que una clase tiene mas de una responsabilidad debemos extraer una nueva clase:

```pascal
TCredito = class
  private
    procedure ActualizarCliente(const aClienteId: Integer);
    procedure AplicarAbono(const aMonto: Currency);
    procedure AplicarCargo(const aMonto: Currency);
    procedure PersistirCambios;
    procedure EnviarCambiosANube;
end;
```

En el ejemplo anterior podemos detectar varios code smells que nos indican que la clase tiene multiples responsabilidades:

* PersistirCambios esta relacionado con la base de datos, esta clase no tiene un constructor o función de configuración donde se le inyecte una clase que se encargue de la base de datos así es que tendrá que hacerlo por ella misma, es decir crear la conexión a la base de datos, crear los queries, etc. **Responsabilidad 1 - Manejar base de datos.**
* ActualizarCliente tiene que trabajar sobre otra clase (en teoría TCliente) que no debería ser responsabilidad de TCredito, un crédito no debe decidir cuando se actualiza un cliente ya que no conoce(no debe conocer) las reglas de negocio que afectan a un Cliente. **Responsabilidad 2 - Modificar Clientes**
* EnviarCambiosANube al igual que con la base de datos debe conocer los datos de la conexión a nube y manejar la interacción. **Responsabilidad 3 - Manejar interacción con nube**
* AplicarAbono y AplicarCargo es la única responsabilidad que sí debe manejar TCredito ya que un crédito conoce las reglas necesarias para aplicar un abono y un cargo. **Responsabilidad 4 - Manejar todo lo relacionado a un crédito**

Una mejor opción seria dividir estas responsabilidades:

```pascal
//Maneja la base de datos
TRepositorioCreditos = class
  public
    procedure AgregarCredito(const aCredito: TCredito);
end;

//Maneja la lógica de negocio de Clientes
TCliente = class
  public
    property Saldo: Currency read fSaldo write SetSaldo;
end; 

//Maneja la interacción con nube
TMessageBus = class
  public
    procedure Publicar(const aMensaje: TMensaje);
    procedure PublicarTexto(const aMensaje: String);
end;

//Maneja la lógica de negocio de Creditos
TCredito = class
  public
    constructor Create(
      const aRepositorio: TRepositorioCredito;
      const aBusMensajes: TMessageBus;
    );
    procedure AplicarAbono(const aMonto: Currency);
end;

//Metodo de TCredito que utiliza las dependencias para que ellas
//hagan el trabajo que les corresponde.
procedure AplicarAbono(const aMonto: Currency);
begin
  Validar.MayorACero(aMonto);
  Validar.NumeroPositivo(Saldo - aMonto);

  Saldo := Saldo - aMonto;
  
  Repositorio.AgregarCredito(Self);
  BusMensajes.PublicarTexto('Credito Actualizado');
  ....
end;
```

El ejemplo anterior no es la mejor opción que existe pero sirve para demostrar el principio de responsabilidad única (principio SRP).

### Debe ser pequeña

una clase demasiado grande es un code smell, nos indica que probablemente la clase hace demasiadas cosas (tiene más de una responsabilidad), a esto se le conoce como **God Class anti-pattern**.