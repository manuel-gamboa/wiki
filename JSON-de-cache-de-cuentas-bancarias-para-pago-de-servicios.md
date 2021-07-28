Para cada proveedor de pago de servicios se creará un registro dentro de la tabla `configuración` con la llave `pagoservicios.%nombreproveedor%.cuentasbanco` con el siguiente formato JSON:

```json
{
   "001" : {
      "ID" : "001",
      "Banco" : "BANAMEX",
      "Cuenta" : "",
      "Titular" : "",
      "IDInterno" : "3"
   }
}
```