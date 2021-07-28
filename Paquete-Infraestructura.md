### Namespace Infra

El paquete de infraestructura implementa todas las interfaces definidas en app.infra y los objetos definidos en esta capa son inyectados en los casos de uso(comandos y queries), es importante hacer notar que al implementar interfaces se pueden tener múltiples objetos para una misma interface, esto le permite a la aplicación manejar diferentes tecnologías sin alterar la capa de aplicación, por ejemplo podríamos tener dos implementaciones de base de datos, una para Firebird y otra para sqlite y la aplicación podría funcionar con ambas de manera transparente.

```pascal
unit infra.db.repositorio.firebird.RepositorioCreditos;

uses
  app.infra.Creditos;

type
  TRepositorioCreditosFirebird = class(IRepositorioCreditos)
  end;
__________________________________________________________
unit infra.db.repositorio.sqlite.RepositorioCreditos;

uses
  app.infra.Creditos;

type
  TRepositorioCreditosSqlite = class(IRepositorioCreditos)
  end;

__________________________________________________________
unit App;

procedure Cargar;
var
  repositorioFirebird: TRepositorioCreditosFirebird;
  repositorioSQlite: TRepositorioCreditosSqlite;
  aplicarCargo: TAplicarCargo;
  aplicarCargo2: TAplicarCargo;
begin
  //el caso de uso funciona correctamente en ambos casos;
  aplicarCargo := TAplicarCargo.Create(repositorioFirebird);
  aplicarCargo2 := TAplicarCargo.Create(repositorioSqlite); 
end;
```

### Paquetes Internos

![](https://raw.githubusercontent.com/bambucode/resources/main/infra.png)

1. **db** Contiene las implementaciones relacionadas con la base de datos, es decir los repositorios, estos contienen toda la lógica de interacción con la base de datos, debe existir un repositorio por cada entidad de dominio a menos que la entidad sea parte de otra entidad, por ejemplo puede existir una entidad ArticuloDeVenta pero esta le pertenece a la Venta, no puede existir la primera sin la segunda, en ese caso el repositorio de Venta debe manejar tambien los artículos de venta.

```pascal
type
  TRepositorioCredito = class(IRepositorio)
    ...
  end;

implementation

procedure Agregar(const aCredito: TCredito);
begin
  db.Command(
    Format(
      'insert into Credito(ClienteId, Saldo, Limite, EstaActivo) values(%d, %s, %s, %d)',
      [
        aCredito.ClienteId, 
        CurrToStr(aCredito.Saldo), 
        CurrToStr(aCredito.Limite),
        BoolToInt(aCredito.EstaActivo)
      ]
    );
end;
```

Por ahora este es el unico paquete que se esta utilizando.

2. **Hardware** Contiene las implementaciones relacionadas con dispositivos de hardware como basculas, escáners y cajas de dinero

3. **OS** Contiene las implementaciones relacionadas con el sistema operativo por ejemplo lectura de archivos o interacción con el API de windows.

4. **Broker** Contiene las implementaciones relacionadas con el message broker de sincronización o bus de eventos, inicialmente se supone que toda la comunicación externa será mediante este canal, sin embargo de existir otros medios de comunicación se puede crear la subcapa webservice o extrnal.