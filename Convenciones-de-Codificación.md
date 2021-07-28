
### Funciones y Procedimientos

_**Deben ser pequeños**_: si una función es demasiado larga se complica su análisis.

_**Deben hacer una sola cosa**_: si encontramos una función que hace varias cosas debemos extraer la funcionalidad en multiples funciones, un code smell muy importante es cuando encontramos comentarios que indican lo que hace una parte del código , si encontramos este tipo de comentarios lo mas probable es que tengamos que extraer la funcionalidad.

```pascal
function TMyClass.GenerarComandoGraphQL(
  const aEntidadOriginal: TEntidadDTO;
  const aEntidadModificada): GraphQLCommand;
begin
  //Obtener las diferencias entre ambas entidades
  ...
  ...
  ...

  //Validar si no existe una entidad anidada en las diferencias
  ...
  ...
  ...

  //Generar string con el comando
  ...
  ...
end
```

En el caso mostrado arriba cada una de las partes del código debería ser una función diferente, hay que extraer cada parte en su propia función.

_**Debe usar pocos argumentos**_: El numero de argumentos máximo debe ser 3, si se necesitan mas debemos crear un objeto que contenga todos los datos, esta regla y todas las demás pueden romperse, si es totalmente necesario usar mas de 3 argumentos sin usar una clase se puede hacer, pero deberán ser muy pocos los casos que requieran romper las convenciones.

```pascal
procedure TMyClass.GuadarCliente(
  const aNombre: String;
  const aDomicilio: String;
  const aEdad: Integer;
  const aNacionalidad: String);

//una mejor opción es agrupar los datos
procedure TMyClass.GuardarCliente(const aCliente: TClienteDTO);
```

### Clases

_**Debe tener una sola responsabilidad**_: si encontramos que una clase tiene mas de una responsabilidad debemos extraer una nueva clase:

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
* **PersistirCambios** esta relacionado con la base de datos, esta clase no tiene un constructor o función de configuración donde se le inyecte una clase que se encargue de la base de datos así es que tendrá que hacerlo por ella misma, es decir crear la conexión a la base de datos, crear los queries y enviarlos. **Responsabilidad 1 - Manejar base de datos.**
* **ActualizarCliente** tiene que trabajar sobre otra clase (en teoría TCliente) que no debería ser responsabilidad de TCredito, un crédito no debe decidir cuando se actualiza un cliente ya que no conoce (no debe conocer) las reglas de negocio que afectan a un Cliente. **Responsabilidad 2 - Modificar Clientes, algo de lo cual no tiene conocimiento**
* **EnviarCambiosANube** al igual que con la base de datos debe conocer los datos de la conexión a nube y manejar la interacción. **Responsabilidad 3 - Manejar interacción con nube**
* **AplicarAbono** y **AplicarCargo** es la única responsabilidad que sí debe manejar TCredito ya que un crédito conoce las reglas necesarias para aplicar un abono y un cargo. **Responsabilidad 4 - Manejar todo lo relacionado a un crédito**

Una mejor opción seria dividir estas responsabilidades:

```pascal
TRepositorioCreditos = class
  public
    procedure AgregarCredito(const aCredito: TCredito);
end;

TCliente = class
  public
    property Saldo: Currency read fSaldo write SetSaldo;
end;

TMessageBus = class
  public
    procedure Publicar(const aMensaje: TMensaje);
    procedure PublicarTexto(const aMensaje: String);
end;

TCredito = class
  public
    constructor Create(
      const aRepositorio: TRepositorioCredito;
      const aBusMensajes: TMessageBus;
    );
    procedure AplicarAbono(const aMonto: Currency);
end;

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

_**Debe ser pequeña**_: una clase demasiado grande es un code smell, nos indica que probablemente hay una falla de diseño o que la clase hace demasiadas cosas (tiene más de una responsabilidad), a esto se le conoce como **God Class anti-pattern**.

_**Debe manejar los invariants de manera adecuada**_: Si una clase tiene dependencias de las cuales no controla su ciclo de vida para evitar tener que estar verificando en todos lados si no es nula, tenemos dos opciones.

* Por convención si las dependencias son Singleton nunca deben ser destruidas hasta que el programa se cierre , también podemos usar Trasients (una nueva instancia cada vez que se resuelve) y destruirlas cuando terminemos de usarlas.
* Usar propiedades para validar que la dependencia exista.

```pascal
//dependencia Singleton.
//si decidimos por convención que todos los repositorios son singleton entonces podemos estar seguros
//que fRepositorio nunca va a ser nulo ya que se destruye solo cuando el programa se cierra.
constructor Create(const aRepositorio: TRepositorioCreditos);
begin
  fRepositorio := aRepositorio;
end;

procedure PersistirCambios;
begin
  //No hay necesidad de hacer un Assert.NotNull(fRepositorio);
  fRepositorio.AgregarCredito(...);
end;

//dependencias Transient.
//por convención si usamos trasients nunca se debe usar un mismo objeto en dos lugares al mismo tiempo
//esto no es tan difícil como suena si se tiene un buen diseño pero para asegurarnos podemos 
//usar propiedades
property Repositorio: TRepositorioCreditos read GetRepositorio;

constructor Create(const aRepositorio: TRepositorioCreditos);
begin
  fRepositorio := aRepositorio;
end;

//de esta manera solo se hace el Assert en un solo lado y no se tiene que repetir en cada función.
function GetRepositorio: TRepositorioCreditos;
begin
  Assert.NotNull(fRepositorio);
  Result := fRepositorio;
end;

procedure PersistirCambios;
begin
  //Usamos la propiedad
  Repositorio.AgregarCredito(...);
end;
```

En el ejemplo anterior usamos una propiedad pero si el programa esta bien diseñado y se tiene una estructura jerárquica clara no es necesario usarlas, queda a consideración del programador.



|                 | Regla.                        |   
| --------------- | -------------------------     |
| **Comentarios** |  |
| Malos Comentarios | Los comentarios no deben explicar Que se esta haciendo , pueden explicar Porque, pero como regla general si un código necesita ser explicado con comentarios puede ser que este mal diseñado o no cumpla con las otras convenciones |
| Código Muerto | no debemos dejar código documentado, si de manera personal te sirve en lo que estas haciendo modificaciones debes removerlo antes de hacer push |
| **Excepciones** |  |
| Custom Exceptions | Usa excepciones que describan elementos de Eleventa y no las Default, por ejemplo en lugar de usar una `Exception` puedes usar una `EDatabaseException` o `EValidacionVentasException` |
| Pre condiciones | Siempre que una pre condición no se cumpla debes lanzar una excepción, ejemplo: `if aSaldo < 0 then raise EBusinessRuleValidationException.Create('El saldo no puede ser negativo')` |
| **Concurrencia** |  |
|  | Todo el código que se mande llamar desde la interface de usuario (Formas VCL) debe ejecutarse de manera asíncrona  |

Es Recomendable que todo desarrollador de Eleventa Desktop lea el siguiente libro que contiene todas estas reglas explicadas de manera detallada y con ejemplos.

[Construyendo Software Fácil de Mantener](https://3.basecamp.com/3077179/buckets/1219561/uploads/2931978632)