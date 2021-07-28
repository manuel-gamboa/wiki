QA Audits es una herramienta de análisis estético incluida en Delphi, para acceder a ella solo tienes que ir al menú Project>QA Audits, en la carpeta de resources>audits del repositorio de ejemplo encontraras el archivo de configuración que puedes cargar en la ventana principal de Audits.

Audits proporciona mucha información, lamentablemente no sustituye a un Linter y al dar muchos falsos positivos, ademas de producir una lista gigantesca de resultados en eleventa no la usaremos de manera automatizada como parte del Pipeline de continua, sin embargo ofrece información valiosa y deberíamos ejecutarla de vez en cuando en nuestros equipos, un ejemplo de esa información es:

* Variables, constantes y métodos no usados, así podemos identificar que no se usa y eliminarlo.
* Tamaño de clases, si las clases son muy grandes podemos decidir dividirlas en múltiples clases mas pequeñas.
* Código Duplicado, encuentra bloques de código repetido.