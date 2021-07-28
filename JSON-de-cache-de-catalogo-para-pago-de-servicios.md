Para cada proveedor de pago de servicios se creará un registro dentro de la tabla `cache` con la llave `pagoservicios.%nombreproveedor%.catalogo` con el siguiente formato JSON:

```json
{
	"ADT": {
		"TieneCodigoDeBarras": "f",
		"Minimo": "0",
		"DebeConsultarSaldo": "t",
		"Nombre": "ADT",
		"AceptaPagoParcial": "t",
		"MontoSoloLectura": "f",
		"MensajeDelProveedor": "Prueba de ProviderMessage",
		"Codigo": "ADT",
		"AceptaVencidos": "t"
	},
  ...
}
```