Desde el release 3.30 (pago de servicios) se estandarizó la librería `TLog` ubicada en `LibreriasDelphi\Varios\uLog.pas`. Dicha clase es independiente de eleventa o de alguna otra librería y solo depende de `CodeSite`. El propósito es usar dicha librería para realizar cualquier logeo en eleventa.

El método a usar será:

``` class procedure Logear(const aMensaje: String;
                             const aCategoria: String = '';
                             const aTipo: TTipoErrorLog = tlNormal;
                             const aColor: TColor = clBlack);```

El parámetro de `aCategoria` deberá de incluir el nombre de la clase donde se origina el log o bien el nombre del módulo de eleventa.

En el parámetro de color estaremos usando los siguientes colores para distinguir:

* clBlack : Color default.
* clRed : Mensaje de error o excepciones (por default)
* clTeal : Queries de SQL ya sean de ClaseDB o Aurelius.
* clSilver : Mensajes de baja importancia.

Propuestas (LH):
* clNavy : Pago de Servicios
* clWebRoyalBlue: Recargas
* clWebOrangeRed : Warnings
* clMaroon : Lector Módulos
* clPurple : Mantenimiento BD
* clGreen  : Inventarios
* clWebSaddleBrown : Facturación
* clWebDarkViolet : Clientes
* clWebViolet : Actualizador masivo
* clWebMediumVioletRed : Nube
* clOlive : Importador
* clWebOliveDrab : Devoluciones
* clWebIndigo : Turnos