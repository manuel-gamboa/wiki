Para cada proveedor de pago de servicios se creará un registro dentro de la tabla `configuración` con la llave `pagoservicios.%nombreproveedor%.cachecampos` con el siguiente formato JSON:

```json
{
   "CODIGOTELMEX" : {
    "DV" : {
      "NombreInterno" : "DV",
      "Interfase" : "Digito Verificador",
      "Regex" : "^[0-9]*$"
    },
    "OTR" : {
      "NombreInterno" : "OTR",
      "Interfase" : "Otro campo dinamico",
      "Regex" : ""
    }
  },
  "CODIGOCFE" : {
    "NUMMEDIDOR" : {
      "NombreInterno" : "NUMMEDIDOR",
      "Interfase" : "Numero de medidor",
      "Regex" : "^[0-9]*$"
    }
  }
}
```