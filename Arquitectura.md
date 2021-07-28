Intentaremos desarrollar toda la funcionalidad nueva usando una arquitectura de software llamada "[Arquitectura Limpia](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)", a continuación veremos la estructura de proyecto recomendada y las reglas que debemos seguir para cumplir con esta arquitectura.

### Código
Todo el código nuevo debe ir dentro de la carpeta src , la estructura de carpetas es la siguiente:

* Entities
* UseCases
* DataProviders
* DataTransfer
* adapters
* shared

**Entities**: en esta carpeta van las entidades de negocio como Venta, Credito, Abono, Factura, las reglas de negocio que aplican a cada entidad son parte del comportamiento de la entidad (métodos), por ejemplo en el caso de un credito, una regla de negocio es que tiene que pertenecerle a un cliente por lo tanto no puede existir un credito sin un cliente, otra regla es que el limite de créditos no puede ser negativo, también podría ser que el saldo no puede ser negativo aunque en el caso de eleventa el saldo si puede ser negativo , todas estas reglas se tienen que codificar dentro de la entidad.

```pascal 
procedure SetLimiteCredito(const Value: Currency);
begin
  if not (Value >= 0.00) then
    raise EValidationError.Create('El limite de crédito no puede ser negativo');

  fLimiteCredito := Value;
end;
```

**UseCases**: un caso de uso es un proceso que involucra a las entidades , por ejemplo Crear un Crédito nuevo, ademas de las reglas de negocio de la entidad , crear un crédito involucra mas pasos, después de validar las reglas se debe almacenar el la base de datos el crédito y después generar un registro de la transacción en el log de transacciones(movimientos) , los use cases orquestan los pasos sin llevarlos a cabo , el use case no hace queries a la base de datos , solo el ordena a algún objeto que lo haga , el use case es el algoritmo principal en el podemos ver todos los pasos necesarios para realizar una acción.

```pascal 
function Ejecutar(const aRequest: TRequestNuevoCredito): TResponseNuevoCredito;
var Credito: TCreditoEntity;
begin
  Result := TResponseNuevoCredito.Create;

  Credito := TCreditoEntity.Nuevo(aRequest.ClienteId, aRequest.LimiteCredito);
  GuardarEnBaseDeDatos(Credito);
  GuardarTransaccion(aRequest);  

  Result.Success := True;
  Result.CreditoId := Credito.Id;
end;
```

**DataProviders**: son Interfaces que definen lo que los casos de uso necesitan para realizar su trabajo, en el ejemplo anterior el caso de uso necesita guardar datos en una base de datos , la definición(contrato) del objeto que guardará los datos se hace mediante una interface, le llamamos proveedor de datos por que nos permite obtener o interactuar con datos del exterior.

```pascal 
ICreditosDatabaseDataProvider = Interface
  function GuardarCredito(const aDatosDelCredito): Boolean;
end;
```

**DataTransfer**: Son objetos sin comportamiento (sin métodos) que solo nos sirven para transportar datos entre las diferentes capas del programa, pueden ser DTOs (data transfer objects)o cualquier otro tipo de objeto que nos permita comunicar peticiones y resultados entre las capas.

```pascal 
type
  //Objeto que se usa para hacer Request desde el exterior hacia los casos de uso
  TNuevoCreditoRequest = class
    public
      ClienteId: Integer;
      LimiteCredito: Currency;
      UserId: Integer;
  end;
```

**Adapters**: Son objetos que implementan las interfaces definidas en data providers, estos se inyectaran en los casos de uso y son intercambiables, podemos tener un adaptador para Firebird y otro para SQLite, Mongo o cualquier otro e intercambiarlos en runtime según las necesidades del cliente.

```pascal 
type
  //Implementa un dataProvider
  TRepositorioCreditos = class(ICreditosDatabaseDataProvider)
    public
      function CrearCredito(const aCredito: TCreditoDTO): TCreditoDTO;
  end;

implementation
//recibe un objeto del namespace datatransfer
//trabaja con la base de datos real
function TRepositorioCreditos.CrearCredito(const aCredito: TCreditoDTO): TCreditoDTO; 
begin
  Fdatabase.ExecuteCommand('insert into Credito(....) values(...)');
  ...  
end; 
```

**Shared**: lo usamos para poner clases que se usan en las distintas capas o que no encajan en alguna de las otras definiciones

Dentro de cada una de estas carpetas creamos sub carpetas para cada uno de los módulos.

En Delphi no existe el concepto de paquete o namespace pero por convencion debemos programar imaginando que la estructura básica del programa es la siguiente:

![](https://i.imgur.com/kuC7pKR.jpg)

### Regla de dependencias
![](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

Como se puede observar en la imagen cada circulo solo puede depender de su circulo interno, es decir entidades no depende de nadie, casos de uso solo puede depender de entidades, los adaptadores solo pueden depender de los casos de uso (y otras estructuras que se encuentran en la misma capa que los casos de uso como los dataproviders).

### Pruebas
otra carpeta raiz (igual que src) será test_ (el guion bajo es por que ya existe otra carpeta Test que no se ajusta a la nueva estructura) , dentro de test_ debe existir la misma estructura de carpetas que dentro de src, pero no todos las clases de src tendrán su prueba dentro de test_, por ejemplo las clases de datatransfer no necesitan ser probadas, dataprovider son solo interfaces por lo que no pueden ser probadas, etc., lo que si debe ser probado son todas las entidades (Entities) y todos los casos de uso (UseCases), también en pruebas de integración deben ser probados todos los adaptadores (Adapters).

La convención para nombrar las pruebas es:Ruta + Nombre del Objeto que se esta probando + Spec , por ejemplo:

`entities.creditos.CreditoSpec o usecases.creditos.NuevoCreditoSpec` 

Para pruebas de integración:

`entities.usecases.creditos.NuevoCreditoIntegrationSpec`

Tenemos que marcar las pruebas de integración con el atributo [Category('integration')]:

```pascal
[TestFixture]
[Category('integration')]
TAsignarVentaACreidtoIntegrationSpec = class(TObject) 
```

Lo anterior con el objetivo de poder filtrar las pruebas, lo cual seria útil si algún día el numero de pruebas es muy grande:

`testproject.exe --exclude:"integration"`

Se recomienda solo probar el comportamiento esperado y no todos los métodos, hay publicaciones que dicen que tenemos que aspirar a tener el 100% del código probado y que incluso usemos herramientas de "Test Coverage" y que solo es aceptable una cobertura del 80% mínimo, pero en realidad al probar el API de una clase (sus métodos públicos) estamos probando todo lo que puede desencadenar un error.

Debemos llamar a los test cases como lo que esperamos que la clase haga o como debería de comportarse, por ejemplo:

`procedure DebeLanzarErrorDeValidacionSiElLimiteDeCreditoEsNegativo`

para usar test cases parametrizados se recomienza usar "Faker" y variables globales en lugar de argumentos de procedure, principalmente por que Faker produce valores aleatorios y eso es mejor para probar que valores preestablecidos por el usuario , al ser aleatorio es mas probable que falle y estaremos probando un valor distinto cada vez que se corra la prueba, sin embargo cualquiera de los dos métodos es correcto:

```pascal 
//argumentos
[Test]
[TestCase('Caso1','1,-1.00')]
[TestCase('Caso2','3,-20.00')]
procedure DebeLanzarErrorSiLimiteDeCreditoEsNegativo(const IdCliente: Integer; const Limite : Currency);

//Faker, cada vez que corres la prueba se usa un valor aleatorio.
procedure TNuevoCreditoSpec.Setup;
begin
  RepositorioCreditos := TMock<ICreditosDataProvider>.Create;
  RepositorioTransacciones := TMock<ITransaccionesDataProvider>.Create;

  IdUsuario := TFaker.integer();
  IdCliente := TFaker.integer();
  IdCredito := TFaker.integer();
  LimiteCredito := TFaker.currency();
end;
```

### En Progreso....