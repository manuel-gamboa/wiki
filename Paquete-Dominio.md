En este paquete se encuentra la lógica de negocio, esta compuesto por entidades de dominio y servicios.

![](https://raw.githubusercontent.com/bambucode/resources/main/dominio.png)


1. **Entidad** Las entidades representan sujetos, por ejemplo un software que maneja créditos puede tener las siguientes entidades:

* Credito
* Cliente
* Abono
* Cargo
* Usuario
* Venta
* Historial de Movimientos

Cada una de estas entidades tiene propiedades y lleva a cabo las operaciones de negocio que le corresponden, tomemos como ejemplo la entidad Crédito:

```pascal
type
  TCredito = class
    public
      property ClienteId: Integer read fClienteId write SetClienteId;
      property Limite: Currency read fLimite write SetLimite;
      property Saldo: Currency read fSaldo write SetSaldo;

      procedure AplicarCargo(const aMonto: Currency);
      procedure AplicarAbono(const aMonto: Currency);
      procedure Suspender;
  end;
```
Cómo podemos ver es muy parecido a un DTO pero la diferencia es que la entidad si tiene métodos (comportamiento) y sus propiedades usan Setters que sirven para validar las reglas de negocio.

```pascal
//Setter
procedure TCredito.SetSaldo(const Value: Currency);
begin
  //regla de negocio: el saldo nunca puede ser negativo
  Assert(Value >= 0, 'El saldo no puede ser negativo'); 

  fSaldo := Value;
end;

//Operación
procedure TCredito.AplicarCargo(const aMonto: Currency);
var 
  Disponible: Currency;
begin
  //regla de negocio: no se puede aplicar el cargo si es superior al crédito disponible
  Disponible := Limite - Saldo;
  Assert(aMonto <= Disponible, 'El cargo supera al crédito disponible');

  Saldo := Saldo + aMonto;
end;
```

Las entidades se aseguran que las reglas de negocio sean aplicadas, de esta manera nuestro programa solo aceptara información valida.

2. **Servicios** Contienen logica de negocio que no corresponde a una entidad en especifico, al igual que los servicios de aplicación son objetos raros y el namespace se deja para uso futuro.