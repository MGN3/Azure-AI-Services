# Supervisión de Cognitive Services

## Aletas
Se pueden crear alertas para avisar sobre diferentes sucesos
Estableciendo un "signal" o condición para que la alerta se dispare.

También hay otros elementos configurables sobre la alerta, como el scope, varias alertas?, etiquetas...

Cuando sucedan los eventos de la alerta se quedarán registrados para poder revisar los registros.

Estos registros pueden exportarse.

En el laboratorio hice pruebas con el signal Keys. Cada vez que se solicitaban las claves, se creaba un log file con la alerta.

## Metricas

Estas sirven para llevar un control de telemetría y de datos de uso, salud, costes... etc.

Mediante las métricas se pueden controlar diversas cuestiones relativas a los servicios de Azure.

### Registros de diagnóstico
Azure Event Hub puede ser el destino de la telemetría. O también conectarlo con una solución de terceros.

Pero parece ser recomendable/necesario usar:

	1- Azure Log Analytics Consulta y visualización en Azure Portal.
	2- Azure Storage: Datos en la nube donde almacenar log files(PUEDEN exportarse)
	
Es necesario crear estos recursos para poder relacionarlos con las métricas de un servicio Azure.

**Importante** El recurso/cuenta Azure Storage debe estar en la misma región donde se ubica el servicio a medir.

Es posible visualizar los datos a través de graficos sobre los que aplicamos parámetros como:
	- Servicio a medir
	- Periodo temporal
	- Parámetros(count, sum, mas...)

### Panel
Se pueden visualizar más de un gráfico en un mismo panel, incluso de diferentes servicios.


## Extra:
En el caso de desplegar servicios Azure en local a través de contenedores locales por ejemplo, el coste de los mismos se calcula igual que si se utilizara desde la nube.

Azure obliga a enviar métricas de uso y otros datos a fin de calcular los costes derivados del uso del servicio.
En caso de no realizar esta conexión no se podrá utilizar el servicio.

Hay algunos servicios no disponibles en local, como detección de caras.

