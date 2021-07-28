A partir de la versión 3.30.XX de eleventa, se soportan llamados a eleventa a través de URI schemes con el propósito de mandar configurar eleventa desde alguna liga en particular, principalmente por correo electrónico.

Sintaxis de configuración de pago de servicios

Para mandar configurar eleventa con las credenciales de pago de servicios, se deberá mandar una liga de la forma:
`eleventa:configuracion|PagoDeServicios|A|B|C|D|E|F|G`

Donde:
* A es: Nombre de Proveedor
* B es: Usuario
* C es: Contraseña
* D es: Canal (ePago)
* E es: UsuariosSFTP
* F es: ClaveSFTP
* G es: ASC

Para tener una mayor seguridad, se encriptan los datos en base64 a partir de esta cadena: `PagoDeServicios|A|B|C|D|E|F|G`quedando como ejemplo: `eleventa:configuracion|UGFnb0RlU2VydmljaW9zfGVQYWdvfGFnZW5lcmljb3xQcnVlYmEyMDE1JHxERVYwMDF8VXN1YXJpb1NGVFB8Q2xhdmVTRlRQfEFTQw==`
 