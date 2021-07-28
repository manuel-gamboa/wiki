### Namespace app

Este paquete contiene las clases responsables de proporcionar servicios a la interface de usuario, contiene principalmente los casos de uso, es decir todas las acciones que el programa puede realizar, podemos describirla como un catalogo de todas las acciones (comandos) y consultas (queries).

```pascal
//Forma de creditos
{ 
  La forma de crédito utiliza los servicios del paquete aplicación, 
  el principal servicio son los casos de uso,estos se dividen en comandos y queries
}
uses
  app.command.creditos.AplicarCargo;
  app.query.creditos.ObtenerReporteDeCredito;

procedure TFormaCredito.btnAplicarCargoClick;
var
  aplicarCargo: TAplicarCargo;
  obtenerReporteDeCredito: TObtenerReporteDeCredito;
begin
  aplicarCargo := TAplicarCargo.Create;
  obtenerReporteDeCredito := TObtenerReporteDeCredito.Create;

  aplicarCargo.Monto := edtCargo.Text;
  aplicarCargo.ClienteId := ClienteSeleccionado.Id;
  //Internamente el comando valida las reglas, almacena los cambios en db y hace todo lo necesario
  aplicarCargo.Ejecutar(); 

  obtenerReporteDeCredito.Ejecutar(ClienteSeleccionado.Id);
  
  lblCreditoDisponible.Caption := query.Credito.Disponible.ToString();
  lblFechaUltimoAbono.Caption := query.Credito.FechaUltimoAbono.ToString();
end;
```

Cómo podemos ver la forma (interface de usuario) no tiene ningúna lógica de base de datos, toda la lógica ya no es su responsabilidad solo consume los servicios del paquete aplicación.

### Paquetes Internos

![](https://raw.githubusercontent.com/bambucode/resources/main/app.png)

1. **Command** Contiene todas las acciones(comandos) que puede realizar el modulo, por ejemplo en el caso del modulo de créditos los comandos podrían ser:
```pascal
app.command.creditos.AplicarCargo;
app.command.creditos.AplicarAbono;
app.command.creditos.ActivarCredito;
app.command.creditos.SuspenderCredito;
app.command.creditos.ModificarLimite;
```

_**Nota**: es la primera vez que hablamos de módulos, todos los paquetes de segundo nivel(por ejemplo app.command o dominio.entidad) están divididos en módulos, un modulo corresponde a un feature de eleventa por ejemplo(Créditos, Facturación, Ventas, Clientes), para créditos el namespace es app.command.creditos_

Un comando posee propiedades que representan los argumentos necesarios para poder llevar a cabo su trabajo y un solo método publico llamado Ejecutar, es importante mencionar que todas las dependencias necesarias que sean externas al paquete aplicación deben ser inyectadas en el constructor.

```pascal
type
  TAplicarCargo = class
    public
      property Monto: Currency read fMonto write setMonto;
      property CreditoId: Integer read fCreditoId write setCreditoId;
      
      constructor Create(const aRepositorioCredito: IRepositorioCredito);
      procedure Ejecutar;
  end;
```

Los comandos son **Transitorios** es decir que se crean y destruyen cada vez que se necesitan.

```pascal
procedure TFormaCredito.BtnAplicarCargoClick;
begin
  try
    comando.Ejecutar;
    MostrarAviso('Cargo aplicado');
  finally
    FreeAndNil(comando);
  end;
end; 
```

Las responsabilidades de un comando son **orquestar** las acciones necesarias para llevar a cabo el comando y hacer algunas validaciones de los argumentos que se le pasan desde el exterior, hay que hacer énfasis en la palabra orquestar ya que el comando no lleva a cabo por si mismo todas las acciones, solo manda llamar a los objetos que deben llevarlas a cabo, al comando se le inyectan las dependencias necesarias que son las que realizan el trabajo.

```pascal
type
  TAplicarCargo = class
  public
    property ClienteId: Integer;
    property Cargo: Currency;

    constructor Create(const aRepositorio: IRepositorio);
    procedure Ejecutar;
  end;

implementation

procedure Ejecutar;
var
  credito: TCredito;
begin
  credito := fRepositorioCredito.Obtener(CreditoId);
  credito.AplicarCargo(Cargo);  //el comando no aplica el cargo, lo hace la entidad credito

  fRepositorioCredito.Actualizar(credito); //el comando no actualiza, lo hace el repositorio  
end;
```

2. **Query** Contiene todas las consultas que se pueden realizar para el modulo:
```pascal
app.query.creditos.ObtenerCreditoParaCliente;
app.query.creditos.ObtenerListadoDeAbonos;
app.query.creditos.ObtenerReporteDeCredito;
app.query.creditos.ObtenerAbonosVencidos;
```

Un Query puede recibir argumentos necesarios para la búsqueda en su método ejecutar y posee propiedades que representan el resultado de la búsqueda, al igual que los comandos recibe las dependencias externas en su constructor y son objetos transitorios.

```pascal
type
  //se pueden usar propiedades
  TObtenerAbonosVencidos = class
    public
      property Abonos: IList<AbonoDTO> read fAbonos;
      
      constructor Create(const aRepositorioCredito: IRepositorioCredito);
      procedure Ejecutar(const Periodo: TPeriodo);
  end;
  
  //o retornar un objeto que contiene todas las propiedades
  TObtenerReporteDeCredito = class
    public      
      constructor Create(const aRepositorioCredito: IRepositorioCredito);
      function Ejecutar(const Periodo: TPeriodo): TReporteDeCreditoDTO;
  end;
```

3. **DTO** Contiene "Data Transfer Objects", estos objetos no tienen comportamiento (métodos) son un conjunto de propiedades similar a un Record y se usan para agrupar datos y transportarlos entre las distintas capas.

```pascal
type
  TDatosCredito = class
    public
      property ClienteId: Integer read fClienteId write fClienteId;
      property Saldo: Currency read fSaldo write fSaldo;
      property Limite: Currency read fLimite write fLimite;
  end;
```

4. **Map** Contiene los mapeadores, son objetos que mapean entidades de dominio a DTOs y viceversa, son necesarios ya que este proceso se repite constantemente.

```pascal
function TCreditoMapper.DomainToData(const aCredito: TCredito): TDatosCredito;
var
  Mapper: TAutoMapper<TCredito,TDatosCredito>;
begin
  Result := TDatosCredito.Create;
  Mapper := TAutoMapper<TVenta,TDatosVenta>.Create;

  Result := Mapper.Map(aCredito);
end;

//para los mapeos mas sencillos como el de arriba existe una clase genérica para que no tengamos que crear
//un mapper para cada entidad.
var
  mapper: TBasicMapper<TDomain, TData>;
  venta: TVenta; //entidad de dominio
  datosVenta: TDatosVenta; // Data transfer object
begin
  venta := mapper.DataToDomain(datosVenta);
```

5. **Infra** Contiene las interfaces de todas las dependencias externas que necesita la aplicación para funcionar, por ejemplo los repositorios de base de datos, existe un archivo infra por cada modulo.

```pascal
unit app.infra.Credito;

type
  IRepositorioCredito = interface
    function Agregar(const aCredito: TCredito): TCredito;
    function Eliminar(const aCreditoId: Integer): Boolean; 
  end;

  IRepositorioTransacciones = interface
    ...
```

6. **Servicio** Contiene clases auxiliares que ayudan a los casos de uso, si encontramos tareas que se repiten en varios casos de uso deberíamos extraer esa tarea en un servicio, es el tipo de objeto mas raro y la mayoría de los módulos no tienen servicios ya que muchas de las tareas repetidas se pueden modelar como un caso de uso adicional, sin embargo dejamos el namespace por si se usa en el futuro.

```pascal
type
  TTransaccionManager = class
    public
      procedure RegistrarTransaccion(...);
  end;
```