# Aprovisionamiento

Para que una API consuma los servicios CS, hay dos opciones:

## Recurso de varios servicios unificado:
Language, Computer Vision, Voz u otros en un **único** recurso.

Ventaja: 
Un único conjunto de credenciales de acceso, punto de conexión y punto de facturación para todos los servicios.

## Recurso de un único servicio:
Puntos de conexión independientes, administrandolos de forma individual.

Ventajas: 
1- Generalmente tienen opción de uso **gratis**.
2- Útil para crear servicios por región geográfica específica.
3- Configuración específica para cada tipo de servicio y clientes.
4- Versátil y personalizable.

## Recursos de entrenamiento y predicción
Algunos CS ofrecen o **requieren** recursos independientes para entrenar los modelos o servicios.

Ventaja: 
La facturación se lleva a cabo de forma independiente a la facturación del servicio.(Supongo que la finalidad es entrenar a un costo mucho más reducido que al costo por uso).

## Identificación de puntos de conexión y claves.
Requisitos para consumir el servicio a través del punto de conexión en la suscripción Azure:

	1- URL del punto de conexión. Dirección HTTP para acceder a la interfaz REST.

	2- Clave de suscripción.Las aplicaciones clientes deben proporcionar una de las dos claves que se crean. Se pueden volver a generar si es necesario para controlar acceso al recurso.

	3- Ubicación del recurso. La mayoría de SDK usan la URL del punto de conexión para conectarse al servicio, pero algunos requieren una **ubicación**(centro de datos de Azure/región Azure donde se define el recurso);

## API REST

A través de una API REST se consume el servicio desde una aplicación cliente.
Las funciones del servicio reciben y envían datos en JSON a través de solicitudes HTTP(GET, POST, PUT).

Al usarse una API REST con punto de conexión HTTP, cualquier lenguaje capáz de enviar y recibir JSON a través de HTTP puede consumir CS (Postman y cURL pueden ser útiles par alas pruebas).

![CognitiveServicesAPIREST](https://learn.microsoft.com/es-es/training/wwl-data-ai/create-manage-cognitive-services/media/rest-api.png)

## SDK para consumir los servicios.
Abstraen las interfaces REST en la mayoría de servicios CS.
Existe disponibilidad de SDK para:
	1- C# .NET
	2- Python
	3- Node.js
	4- GO
	5- Java
Los SDK incluyen paquetes que se pueden instalar par ausar las bibliotecas específicas del servicio. (la documentación oficial es útil para elegir clases, métodos y parámetros que queremos usar).

# Laboratorio:

Nota

Para completar este ejercicio, necesitará una suscripción a Microsoft Azure. Si aún no tiene una, puede solicitar una prueba gratuita en https://azure.com/free.

Se proporciona una máquina virtual (VM) que contiene las herramientas de cliente que necesita, junto con las instrucciones del ejercicio. Utilice el botón que aparece arriba para abrir la máquina virtual. Hay un número limitado de sesiones simultáneas disponibles; si el entorno hospedado no está disponible, inténtelo de nuevo más adelante.

Como alternativa, si desea usar un entorno de desarrollo en su propio equipo, puede utilizar esta guía de configuración y seguir estas instrucciones del ejercicio. Tenga en cuenta que la guía de configuración está diseñada para varios ejercicios de desarrollo de inteligencia artificial y puede incluir software que no es necesario para este ejercicio específico. Además, debido a la variedad de posibles sistemas operativos y configuraciones de instalación, no podemos brindarle soporte si decide completar el ejercicio en su propio equipo.

Cuando termine el ejercicio, finalice el laboratorio para cerrar la máquina virtual. No olvide volver y completar la prueba de conocimientos para ganar puntos por completar este módulo.

 Sugerencia

Después de completar el ejercicio, si terminó de explorar Azure Cognitive Services, elimine los recursos de Azure que creó durante el ejercicio.

REPOSITORIO PARA CLONAR
/MicrosoftLearning/AI-102-AIEngineer

/////////////// INSTRUCCIONES MAQUINA VIRTUAL
Clone the repository for this course
If you have not already cloned AI-102-AIEngineer code repository to the environment where you're working on this lab, follow these steps to do so. Otherwise, open the cloned folder in Visual Studio Code.

Start Visual Studio Code.

Open the palette (SHIFT+CTRL+P) and run a Git: Clone command to clone the https://github.com/MicrosoftLearning/AI-102-AIEngineer repository to a local folder (it doesn't matter which folder).

When the repository has been cloned, open the folder in Visual Studio Code.

Wait while additional files are installed to support the C# code projects in the repo.

Note: If you are prompted to add required assets to build and debug, select Not Now.

Provision a Cognitive Services resource
Azure Cognitive Services are cloud-based services that encapsulate artificial intelligence capabilities you can incorporate into your applications. You can provision individual cognitive services resources for specific APIs (for example, Language or Computer Vision), or you can provision a general Cognitive Services resource that provides access to multiple cognitive services APIs through a single endpoint and key. In this case, you'll use a single Cognitive Services resource.

Open the Azure portal at https://portal.azure.com, and sign in using the Microsoft account associated with your Azure subscription.
Select the ＋Create a resource button, search for cognitive services, and create a Cognitive Services resource with the following settings:
Subscription: Your Azure subscription
Resource group: Choose or create a resource group (if you are using a restricted subscription, you may not have permission to create a new resource group - use the one provided)
Region: Choose any available region
Name: Enter a unique name
Pricing tier: Standard S0
Select the required checkboxes and create the resource.
Wait for deployment to complete, and then view the deployment details.
Go to the resource and view its Keys and Endpoint page. This page contains the information that you will need to connect to your resource and use it from applications you develop. Specifically:
An HTTP endpoint to which client applications can send requests.
Two keys that can be used for authentication (client applications can use either key to authenticate).
The location where the resource is hosted. This is required for requests to some (but not all) APIs


## Use a REST Interface
The cognitive services APIs are REST-based, so you can consume them by submitting JSON requests over HTTP. In this example, you'll explore a console application that uses the Language REST API to perform language detection; but the basic principle is the same for all of the APIs supported by the Cognitive Services resource.

Note: In this exercise, you can choose to use the REST API from either C# or Python. In the steps below, perform the actions appropriate for your preferred language.

In Visual Studio Code, in the Explorer pane, browse to the 01-getting-started folder and expand the C-Sharp or Python folder depending on your language preference.

View the contents of the rest-client folder, and note that it contains a file for configuration settings:

C#: appsettings.json
Python: .env
Open the configuration file and update the configuration values it contains to reflect the endpoint and an authentication key for your cognitive services resource. Save your changes.

Note that the rest-client folder contains a code file for the client application:

C#: Program.cs
Python: rest-client.py
Open the code file and review the code it contains, noting the following details:

Various namespaces are imported to enable HTTP communication
Code in the Main function retrieves the endpoint and key for your cognitive services resource - these will be used to send REST requests to the Text Analytics service.
The program accepts user input, and uses the GetLanguage function to call the Text Analytics language detection REST API for your cognitive services endpoint to detect the language of the text that was entered.
The request sent to the API consists of a JSON object containing the input data - in this case, a collection of document objects, each of which has an id and text.
The key for your service is included in the request header to authenticate your client application.
The response from the service is a JSON object, which the client application can parse.
Right-click the rest-client folder and open an integrated terminal. Then enter the following language-specific command to run the program:

C#

dotnet run
Python

python rest-client.py
When prompted, enter some text and review the language that is detected by the service, which is returned in the JSON response. For example, try entering "Hello", "Bonjour", and "Gracias".

When you have finished testing the application, enter "quit" to stop the program.


Use an SDK
You can write code that consumes cognitive services REST APIs directly, but there are software development kits (SDKs) for many popular programming languages, including Microsoft C#, Python, and Node.js. Using an SDK can greatly simplify development of applications that consume cognitive services.

In Visual Studio Code, in the Explorer pane, in the 01-getting-started folder, expand the C-Sharp or Python folder depending on your language preference.

Right-click the sdk-client folder and open an integrated terminal. Then install the Text Analytics SDK package by running the appropriate command for your language preference:

C#

dotnet add package Azure.AI.TextAnalytics --version 5.3.0
Python

pip install azure-ai-textanalytics==5.3.0
View the contents of the sdk-client folder, and note that it contains a file for configuration settings:

C#: appsettings.json
Python: .env
Open the configuration file and update the configuration values it contains to reflect the endpoint and an authentication key for your cognitive services resource. Save your changes.

Note that the sdk-client folder contains a code file for the client application:

C#: Program.cs
Python: sdk-client.py
Open the code file and review the code it contains, noting the following details:

The namespace for the SDK you installed is imported
Code in the Main function retrieves the endpoint and key for your cognitive services resource - these will be used with the SDK to create a client for the Text Analytics service.
The GetLanguage function uses the SDK to create a client for the service, and then uses the client to detect the language of the text that was entered.
Return to the integrated terminal for the sdk-client folder, and enter the following command to run the program:

C#

dotnet run
Python

python sdk-client.py
When prompted, enter some text and review the language that is detected by the service. For example, try entering "Goodbye", "Au revoir", and "Hasta la vista".

When you have finished testing the application, enter "quit" to stop the program.

Note: Some languages that require Unicode character sets may not be recognized in this simple console application.

More information
For more information about Azure Cognitive Services, see the Cognitive Services documentation.

### RESUMEN: 
Aprovisionar recursos de Cognitive Services en una suscripción de Azure.
Identificar los puntos de conexión, las claves y las ubicaciones necesarias para consumir un recurso de Cognitive Services.
Usar una API REST para consumir un servicio de Cognitive Services.
Usar un SDK para consumir un servicio de Cognitive Services.
