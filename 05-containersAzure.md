# Implementación de Cognitive Services en Contenedores
Info extra: https://learn.microsoft.com/es-es/azure/ai-services/cognitive-services-container-support

## Contenedores
Un contenedor consta de una aplicación o servicio y los componentes de runtime necesarios para ejecutarlo, a la vez que abstrae el sistema operativo y el hardware subyacentes. En la práctica, esta abstracción da como resultado dos ventajas significativas:

Los contenedores son portables entre hosts, que pueden ejecutar sistemas operativos diferentes o usar hardware diferente, lo que facilita el traslado de una aplicación y todas sus dependencias.
Un único host de contenedor puede admitir varios contenedores aislados, cada uno con su propia configuración específica de runtime, lo que facilita la consolidación de varias aplicaciones que tienen requisitos de configuración diferentes.
Un contenedor se encapsula en una imagen de contenedor que define el software y la configuración que debe admitir. Las imágenes se pueden almacenar en un registro central, como Docker Hub, o bien puede mantener un conjunto de imágenes en su propio registro.

### Implementación de contenedores:
Implementación de contenedores
Para usar un contenedor, normalmente se extrae la imagen de contenedor de un registro y se implementa en un host de contenedor, especificando los valores de configuración necesarios. El host de contenedor puede estar en la nube, en una red privada o en el equipo local. Por ejemplo:

Un servidor de Docker*.
Una instancia de Azure Container (ACI).
Un clúster de Azure Kubernetes Service (AKS).
*Docker es una solución de código abierto para el desarrollo y la administración de contenedores que incluye un motor de servidor que puede usar para hospedar contenedores. Hay versiones del servidor de Docker para sistemas operativos comunes, incluidos Microsoft Windows y Linux.

### Procedimiento general:

Hay imágenes de contenedor para Azure Cognitive Services en Microsoft Container Registry que puede usar para implementar un servicio contenedorizado que encapsula una API individual de un servicio cognitivo.

Para implementar y usar un contenedor de Cognitive Services, deben producirse las tres actividades siguientes:

	1- La imagen de contenedor de la API específica de Cognitive Services que quiere usar se descarga e implementa en un host de contenedor, como un servidor de Docker local, instancia de Azure Container (ACI) o Azure Kubernetes Service (AKS).
	2- Las aplicaciones cliente envían datos al punto de conexión proporcionado por el servicio contenedorizado y recuperan los resultados tal como lo harían desde un recurso en la nube de Cognitive Services en Azure.
	3- Periódicamente, las métricas de uso del servicio contenedorizado se envían a un recurso de Cognitive Services en Azure con el fin de calcular la facturación del servicio.

![esquemaCSyDocker](https://learn.microsoft.com/es-es/training/wwl-data-ai/investigate-container-for-use-cognitive-services/media/cognitive-services-container.png)

**Punto clave sobre facturación**
Incluso cuando se usa un contenedor, debe aprovisionar un recurso de Cognitive Services en Azure con fines de facturación. Las aplicaciones cliente envían sus solicitudes al servicio contenedorizado, lo que significa que los datos potencialmente confidenciales no se envían al punto de conexión de Cognitive Services en Azure, pero el contenedor debe poder conectarse al recurso de Cognitive Services en Azure periódicamente para enviar métricas de uso para la facturación.

### Imágenes de contenedor para cada Cognitive services(a veces +1 imagen por servicio)

Cada contenedor proporciona un subconjunto de funcionalidades de Cognitive Services. Por ejemplo, no todas las características del servicio Language están en un único contenedor. La detección de idioma, la traducción y el análisis de sentimiento se encuentran en imágenes de contenedor independientes. Sin embargo, los pasos de configuración son similares para cada contenedor.

En el servicio Language, cada una de las tres características principales se asigna a una imagen distinta:

Característica -- Imagen

Extracción de frases clave -- mcr.microsoft.com/azure-cognitive-services/keyphrase

Detección de idiomas --	mcr.microsoft.com/azure-cognitive-services/language

Análisis de sentimiento, versión 3 (inglés) --	mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-en

# Requisitos de configuración:
**ApiKey** -- Clave del servicio de Azure Cognitive Services implementado. Se usa para la facturación.

**Facturación** -- URI del punto de conexión del servicio de Azure Cognitive Services implementado. Se usa para la facturación.

**CLUF** -- Valor de aceptación para indicar que acepta la licencia para el contenedor.

### Consumo y resumen
Implementado el servicio, las aplicaciones consumen del punto de conexión de Cognitive Services contenedorizado en lugar de utilizar el punto de conexión predeterminado de Azure. 
La aplicación cliente debe configurarse con el punto de conexión adecuado para el contenedor, pero **no es necesario proporcionar una clave de suscripción para autenticarse**. Puede implementar su propia solución de autenticación y aplicar restricciones de seguridad de red según corresponda para su escenario de aplicación específico.

 
### Laboratorio
Deploy and run a Text Analytics container
Many commonly used cognitive services APIs are available in container images. For a full list, check out the cognitive services documentation. In this exercise, you'll use the container image for the Text Analytics language detection API; but the principles are the same for all of the available images.

In the Azure portal, on the Home page, select the ＋Create a resource button, search for container instances, and create a Container Instances resource with the following settings:

Basics:

Subscription: Your Azure subscription
Resource group: Choose the resource group containing your cognitive services resource
Container name: Enter a unique name
Region: Choose any available region
Image source: Other Registry
Image type: Public
Image: mcr.microsoft.com/azure-cognitive-services/textanalytics/language:latest
OS type: Linux
Size: 1 vcpu, 4 GB memory
Networking:

Networking type: Public
DNS name label: Enter a unique name for the container endpoint
Ports: Change the TCP port from 80 to 5000
Advanced:

Restart policy: On failure

Environment variables:

Mark as secure	Key	Value
Yes	ApiKey	Either key for your cognitive services resource
Yes	Billing	The endpoint URI for your cognitive services resource
No	Eula	accept
Command override: [ ]

Tags:

Don't add any tags
Wait for deployment to complete, and then go to the deployed resource.

Observe the following properties of your container instance resource on its Overview page:

Status: This should be Running.
IP Address: This is the public IP address you can use to access your container instances.
FQDN: This is the fully-qualified domain name of the container instances resource, you can use this to access the container instances instead of the IP address.
Note: In this exercise, you've deployed the cognitive services container image for text translation to an Azure Container Instances (ACI) resource. You can use a similar approach to deploy it to a Docker host on your own computer or network by running the following command (on a single line) to deploy the language detection container to your local Docker instance, replacing <yourEndpoint> and <yourKey> with your endpoint URI and either of the keys for your cognitive services resource.

docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/textanalytics/language Eula=accept Billing=<yourEndpoint> ApiKey=<yourKey>
The command will look for the image on your local machine, and if it doesn't find it there it will pull it from the mcr.microsoft.com image registry and deploy it to your Docker instance. When deployment is complete, the container will start and listen for incoming requests on port 5000.

Use the container
In Visual Studio Code, in the 04-containers folder, open rest-test.cmd and edit the curl command it contains (shown below), replacing <your_ACI_IP_address_or_FQDN> with the IP address or FQDN for your container.

curl -X POST "http://<your_ACI_IP_address_or_FQDN>:5000/text/analytics/v3.0/languages?" -H "Content-Type: application/json" --data-ascii "{'documents':[{'id':1,'text':'Hello world.'},{'id':2,'text':'Salut tout le monde.'}]}"
Save your changes to the script. Note that you do not need to specify the cognitive services endpoint or key - the request is processed by the containerized service. The container in turn communicates periodically with the service in Azure to report usage for billing, but does not send request data.

Right-click the 04-containers folder and open an integrated terminal. Then enter the following command to run the script:

rest-test
Verify that the command returns a JSON document containing information about the language detected in the two input documents (which should be English and French).
