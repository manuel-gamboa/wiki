### Campos (fields) 

Comienzan con la letra f.
 
```pascal
TCredito = class
  private
    fSaldo: Currency; //Bien 
    Saldo: Currency;  //Mal
end;
```

### Argumentos de función o procedimiento

Comienzan con la letra a. 

```pascal
procedure AplicarAbono(const aSaldo: Currency); //Bien 
procedure AplicarCargo(const Saldo: Currency);  //Mal
```

### Métodos 

Debe comenzar con mayúscula, se usa PascalCase, no deben usarse abreviaturas o siglas no comunes, el nombre debe expresar la acción, generalmente se usan verbos.
 
```pascal
function ObtenerHistorialCredito(const aClienteId: Integer); //Bien 
function obtenerHistorialCredito(const aClienteId: Integer); //Mal, debe comenzar con mayúscula
function ObtHistCredito(const aClienteId: Integer);          //Mal, no usar abreviaturas que dificulten entender
```

### Variables locales

Deben comenzar con minúscula, Se usa camelCase, el nombre debe describir lo que representa.

```pascal
procedure AplicarAbono(const aSaldo: Currency);
var
  saldoCredito: Currency; //Bien
  SaldoCredito: Currency; //Mal, debe iniciar con minúscula
  Num, a, tmp: Currency;  //Mal, no se sabe que representa la variable
```

### Constantes

Solamente se usan letras mayusculas separadas con "_".

```pascal
procedure AplicarAbono(const aSaldo: Currency);
const
  SALDO_INICIAL: Currency = 0.00; //Bien  
  saldoInicial: Currency = 0.00;  //Mal, deben usarse solo mayusculas
  Saldo_Inicial: Currency = 0.00; //Mal
```

### Enumeraciones 

El nombre de la enumeración debe Comenzar con mayúscula, para los elementos se usan dos letras como prefijo seguido de mayúscula.

```pascal
type
  FormaDePago = (fpEfectivo, fpCredito); //Bien
  formaDePago = (Efectivo, Credito);     //Mal, debe comenzar con mayusculas y usar prefijos para los valores
```

### Clases

Comienzan con la letra T, se puede omitir la T en casos especiales , por ejemplo en alguna librería en la que la T se siente forzada.

```pascal
type
  TCredito = class  //Bien  
  Credito = class   //Mal 

  Assert = class    //Bien, una clase de uso extensivo puede omitir la T sobre todo si se usara de forma estática
```

### Excepciones

Comienzan con la letra E y terminan con la palabra Exception.

```pascal
type
  EValidationException = class(Exception)  //Bien  
  ValidationError = class(Exception)       //Mal 
```