# Publicación y uso de una app Language Understanding.

Objetivos:

	1- Establecer opciones de configuración de publicación para una aplicación de Language Understanding.
	2 Describir las características de Language Understanding.
	3- Implementar el servicio de Language Understanding como contenedor.

## Funcionalidades principales

- Resumen de textos(documentos y conversaciones) en oraciones clave:
/{ENDPOINT}/text/analytics/{VERSION}/analyze
extractiveSummarizationTasks o ConversationalSummarizationTask.

- Reconocimiento de entidades con nombre:
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
EntityRecognition

- Detección de información de identificación personal:
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
PiiEntityRecognition

- Extracción de palabras clave.
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
KeyPhraseExtraction

- Analisis de opiniones(positivo, negativo) muy util para reseñas, valoraciones...
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
SentimentAnalysis

- Detección de idiomas
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
LanguageDetection

- CLU Reconocimiento del lenguaje conversacional.
/{ENDPOINT}/language/:analyze-conversations?api-version={VERSION}
Conversation. Requermiento: parámetros adicionales en el cuerpo de JSON, incluyendo projectName y deploymentName

- Reconocimiento de entidades con nombre personalizado. Extracción de entidades como partes de un contrato/juicio.
/{ENDPOINT}/language/analyze-text/jobs?api-version={VERSION}
CustomEntityRecognition. Requiere parámetros adicionales en el cuerpo JSON, incluidos los projectName y deploymentName

- Clasificación de texto personalizada. Grupos creados para clasificar documentos o textos(noticias, opinión, política, documento legal, artículo científico, tesis...).
/{ENDPOINT}/language/analyze-text/jobs?api-version={VERSION}
CustomMultiLabelClassification o CustomSingleLabelClassification. Dependiendo de si necesita una clasificación de una o varias etiquetas. Esta característica personalizada también requiere parámetros adicionales en el cuerpo JSON, incluidos los projectName y deploymentName del modelo.

### Util para soporte en webs
- Respuesta a preguntas. Entrenar con documentos tipo FAQ y manuales.
{ENDPOINT}/language/:query-knowledgebases?projectName={PROJECTNAME}&deploymentName={DEPLOYMENTNAME}&api-version={VERSION}
{ENDPOINT}	Punto de conexión para autenticar la solicitud de API. Por ejemplo: myLanguageService.cognitiveservices.azure.com
{PROJECTNAME}	Nombre del proyecto en el que proporcionó documentos como datos para responder a preguntas
{DEPLOYMENTNAME}	Nombre de la implementación
{VERSION}	Número de versión de api del servicio al que desea llamar. Por ejemplo: 2022-05-01

- Características extra/aprendidas. Se puede entrenar al modelo para que extraiga, genere o prediga en base a datos de entrenamiento.

## Contenedor
### Para descargar la imagen en local:
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/language:latest

### Ejecutar el contenedor
docker run --rm -it -p 5000:5000 
--memory 4g 
--cpus 1 \
mcr.microsoft.com/azure-cognitive-services/textanalytics/language \
Eula=accept \
Billing={ENDPOINT} \
ApiKey={API_KEY}

El contenedor proporciona un punto de conexión REST para realizar consultas en una dirección URL local: http://localhost:5000/

# Conclusiones
Punto importante: Creo que no configuré el endpoint correcto en el appsettings.json o en program.cs me faltó algun elemento.

Es lioso que el servicio de Lenguage Understanding tenga un endpoint y el servicio creado con ese servicio de Azure en Lenguage Studio sea diferente.

Además, parece que comparten key, aunque son como dos instancias de un mismo recurso pero en diferentes plataformas.

Aparte de eso, la práctica consiste en:
	- Crear el recurso en Azure.
	- Crear un "recurso" en Language Studio con el servicio Azure.
	- Importar un .json con datos para que detecte intenciones y entidades.
	- Una vez importado, se "despliega".
	- En Language Studio selecciono el recurso y ver URL endpoint.(aparece el endpoint y un -curl).
	- Con todo esto, añado codigo que me dan de la práctica y corro el programa.
	- El programa está entrenado para detectar intenciones como
		1- GetTime
		2- GetDay
		3- GetDate
	- También muestra entidades detectadas, como ciudades...
	- Devuelve una respuesta adecuada...



# Laboratorio: Entrenamiento modelo Clock

## Parte 1
Create Language service resources
If you already have a Language service resource in your Azure subscription, you can use it in this exercise. Otherwise, follow these instructions to create one.

Open the Azure portal at https://portal.azure.com, and sign in using the Microsoft account associated with your Azure subscription.

Select the ＋Create a resource button, search for language service, and create a Language service resource with the following settings:

Default features: All
Custom features: none
Subscription: Your Azure subscription
Resource group: Choose or create a resource group (if you are using a restricted subscription, you may not have permission to create a new resource group - use the one provided)
Region: westus2
Name: Enter a unique name
Pricing tier: S
Responsible AI Notice: select check box to confirm
Wait for the resources to be created. You can view your resource by navigating to the resource group where you created it.

Import, train, and publish a Conversational language understanding model
If you already have a Clock project from a previous lab or exercise, you can use it in this exercise. Otherwise, follow these instructions to create it.

In a new browser tab, open the Language Studio - Preview portal at https://language.cognitive.azure.com.

Sign in using the Microsoft account associated with your Azure subscription. If this is the first time you have signed into the Language Service portal, you may need to grant the app some permissions to access your account details. Then complete the Welcome steps by selecting your Azure subscription and the authoring resource you just created.

Open the Conversational Language Understanding page.

Next to ＋Create new project, select Import. Click Choose File and then browse to the 10b-clu-client-(preview) subfolder in the project folder containing the lab files for this exercise. Select Clock.json, click Open, and then click Done.

If a panel with tips for creating an effective Language service app is displayed, close it.

At the left of the Language Studio portal, select Training jobs to train the app. Click Start a training job, name the model Clock and keep default training mode (Standard) and data splitting. Select Train. Training may take several minutes to complete.

Note: Because the model name Clock is hard-coded in the clock-client code (used later in the lab), capitalize and spell the name exactly as described.

At the left of the Language Studio portal, select Deploying a model and use Add deployment to create deployment for the Clock model that's named production.

Note: Because the deployment name production is hard-coded in the clock-client code (used later in the lab), capitalize and spell the name exactly as described.

The client applications needs the Endpoint URL and Primary key to use your deployed model. After the deployment is complete, to get those parameters, open the Azure portal at https://portal.azure.com, and sign in using the Microsoft account associated with your Azure subscription. On the Search bar, search for Language and select it to choose the Cognitive Services|Language service.

Your Language service resource should be listed, select that resource.

On the left hand menu, under the Resource Management section, select Keys and Endpoint.

Make a copy of your KEY 1 and your Endpoint.

Client applications need information from the prediction URL endpoint and the Language service key to connect to your deployed model and be authenticated.

## Parte 2
Prepare to use the Language service SDK
In this exercise, you'll complete a partially implemented client application that uses the Clock model (published Conversational Language Understanding model) to predict intents from user input and respond appropriately.

Note: You can choose to use the SDK for either .NET or Python. In the steps below, perform the actions appropriate for your preferred language.

In Visual Studio Code, in the Explorer pane, browse to the 10b-clu-client-(preview) folder and expand the C-Sharp or Python folder depending on your language preference.

Right-click the clock-client folder and then select Open in Integrated Terminal. Then install the Conversational Language Service SDK package by running the appropriate command for your language preference:

C#

dotnet add package Azure.AI.Language.Conversations --version 1.0.0
dotnet add package Azure.Core
Python

pip install azure-ai-language-conversations --pre
python -m pip install python-dotenv
python -m pip install python-dateutil
View the contents of the clock-client folder, and note that it contains a file for configuration settings:

C#: appsettings.json
Python: .env
Open the configuration file and update the configuration values it contains to include the Endpoint URL and the Primary key for your Language resource. You can find the required values in the Azure portal or Language Studio as follows:

Azure portal: Open your Language resource. Under Resource Management, select Keys and Endpoint. Copy the KEY 1 and Endpoint values to your configuration settings file.
Language Studio: Open your Clock project. The Language service endpoint can be found on the Deploying a model page under Get prediction URL, and the the Primary key can be found on the Project settings page. The Language service endpoint portion of the Prediction URL ends with .cognitiveservices.azure.com/. For example: https://ai102-langserv.cognitiveservices.azure.com/.
Note that the clock-client folder contains a code file for the client application:

C#: Program.cs
Python: clock-client.py
Open the code file, and at the top, under the existing namespace references, find the comment Import namespaces. Then, under this comment, add the following language-specific code to import the namespaces you will need to use the Language service SDK:

C#

C#
// Import namespaces
using Azure;
using Azure.AI.Language.Conversations;
Python

Python
### Import namespaces
from azure.core.credentials import AzureKeyCredential
from azure.ai.language.conversations import ConversationAnaly

## Parte 3
Get a prediction from the Conversational Language model
Now you're ready to implement code that uses the SDK to get a prediction from your Conversational Language model.

In the Main function, note that code to load the prediction endpoint and key from the configuration file has already been provided. Then find the comment Create a client for the Language service model and add the following code to create a prediction client for your Language Service app:

C#

C#
// Create a client for the Language service model
Uri endpoint = new Uri(predictionEndpoint);
AzureKeyCredential credential = new AzureKeyCredential(predictionKey);

ConversationAnalysisClient client = new ConversationAnalysisClient(endpoint, credential);
Python

Python

### Create a client for the Language service model
client = ConversationAnalysisClient(
    ls_prediction_endpoint, AzureKeyCredential(ls_prediction_key))
Note that the code in the Main function prompts for user input until the user enters "quit". Within this loop, find the comment Call the Language service model to get intent and entities and add the following code:

C#

C#
// Call the Language service model to get intent and entities
var projectName = "Clock";
var deploymentName = "production";
var data = new
{
    analysisInput = new
    {
        conversationItem = new
        {
            text = userText,
            id = "1",
            participantId = "1",
        }
    },
    parameters = new
    {
        projectName,
        deploymentName,
        // Use Utf16CodeUnit for strings in .NET.
        stringIndexType = "Utf16CodeUnit",
    },
    kind = "Conversation",
};
// Send request
Response response = await client.AnalyzeConversationAsync(RequestContent.Create(data));
dynamic conversationalTaskResult = response.Content.ToDynamicFromJson(JsonPropertyNames.CamelCase);
dynamic conversationPrediction = conversationalTaskResult.Result.Prediction;   
var options = new JsonSerializerOptions { WriteIndented = true };
Console.WriteLine(JsonSerializer.Serialize(conversationalTaskResult, options));
Console.WriteLine("--------------------\n");
Console.WriteLine(userText);
var topIntent = "";
if (conversationPrediction.Intents[0].ConfidenceScore > 0.5)
{
    topIntent = conversationPrediction.TopIntent;
}
Python

Python
### Call the Language service model to get intent and entities
cls_project = 'Clock'
deployment_slot = 'production'

with client:
    query = userText
    result = client.analyze_conversation(
        task={
            "kind": "Conversation",
            "analysisInput": {
                "conversationItem": {
                    "participantId": "1",
                    "id": "1",
                    "modality": "text",
                    "language": "en",
                    "text": query
                },
                "isLoggingEnabled": False
            },
            "parameters": {
                "projectName": cls_project,
                "deploymentName": deployment_slot,
                "verbose": True
            }
        }
    )

top_intent = result["result"]["prediction"]["topIntent"]
entities = result["result"]["prediction"]["entities"]

print("view top intent:")
print("\ttop intent: {}".format(result["result"]["prediction"]["topIntent"]))
print("\tcategory: {}".format(result["result"]["prediction"]["intents"][0]["category"]))
print("\tconfidence score: {}\n".format(result["result"]["prediction"]["intents"][0]["confidenceScore"]))

print("view entities:")
for entity in entities:
    print("\tcategory: {}".format(entity["category"]))
    print("\ttext: {}".format(entity["text"]))
    print("\tconfidence score: {}".format(entity["confidenceScore"]))

print("query: {}".format(result["result"]["query"]))
The call to the Language service model returns a prediction/result, which includes the top (most likely) intent as well as any entities that were detected in the input utterance. Your client application must now use that prediction to determine and perform the appropriate action.

Find the comment Apply the appropriate action, and add the following code, which checks for intents supported by the application (GetTime, GetDate, and GetDay) and determines if any relevant entities have been detected, before calling an existing function to produce an appropriate response.

C#

C#
// Apply the appropriate action
switch (topIntent)
{
    case "GetTime":
        var location = "local";           
        // Check for a location entity
        foreach (dynamic entity in conversationPrediction.Entities)
        {
            if (entity.Category == "Location")
            {
                //Console.WriteLine($"Location Confidence: {entity.ConfidenceScore}");
                location = entity.Text;
            }
        }
        // Get the time for the specified location
        string timeResponse = GetTime(location);
        Console.WriteLine(timeResponse);
        break;
    case "GetDay":
        var date = DateTime.Today.ToShortDateString();            
        // Check for a Date entity
        foreach (dynamic entity in conversationPrediction.Entities)
        {
            if (entity.Category == "Date")
            {
                //Console.WriteLine($"Location Confidence: {entity.ConfidenceScore}");
                date = entity.Text;
            }
        }            
        // Get the day for the specified date
        string dayResponse = GetDay(date);
        Console.WriteLine(dayResponse);
        break;
    case "GetDate":
        var day = DateTime.Today.DayOfWeek.ToString();
        // Check for entities            
        // Check for a Weekday entity
        foreach (dynamic entity in conversationPrediction.Entities)
        {
            if (entity.Category == "Weekday")
            {
                //Console.WriteLine($"Location Confidence: {entity.ConfidenceScore}");
                day = entity.Text;
            }
        }          
        // Get the date for the specified day
        string dateResponse = GetDate(day);
        Console.WriteLine(dateResponse);
        break;
    default:
        // Some other intent (for example, "None") was predicted
        Console.WriteLine("Try asking me for the time, the day, or the date.");
        break;
}
Python

Python

### Apply the appropriate action
if top_intent == 'GetTime':
    location = 'local'
    # Check for entities
    if len(entities) > 0:
        # Check for a location entity
        for entity in entities:
            if 'Location' == entity["category"]:
                # ML entities are strings, get the first one
                location = entity["text"]
    # Get the time for the specified location
    print(GetTime(location))

elif top_intent == 'GetDay':
    date_string = date.today().strftime("%m/%d/%Y")
    # Check for entities
    if len(entities) > 0:
        # Check for a Date entity
        for entity in entities:
            if 'Date' == entity["category"]:
                # Regex entities are strings, get the first one
                date_string = entity["text"]
    # Get the day for the specified date
    print(GetDay(date_string))

elif top_intent == 'GetDate':
    day = 'today'
    # Check for entities
    if len(entities) > 0:
        # Check for a Weekday entity
        for entity in entities:
            if 'Weekday' == entity["category"]:
            # List entities are lists
                day = entity["text"]
    # Get the date for the specified day
    print(GetDate(day))

else:
    # Some other intent (for example, "None") was predicted
    print('Try asking me for the time, the day, or the date.')
Save your changes and return to the integrated terminal for the clock-client folder, and enter the following command to run the program:

C#

dotnet run
Python

python clock-client.py
When prompted, enter utterances to test the application. For example, try:

Hello

What time is it?

What's the time in London?

What's the date?

What date is Sunday?

What day is it?

What day is 01/01/2025?

Note: The logic in the application is deliberately simple, and has a number of limitations. For example, when getting the time, only a restricted set of cities is supported and daylight savings time is ignored. The goal is to see an example of a typical pattern for using Language Service in which your application must:

Connect to a prediction endpoint.
Submit an utterance to get a prediction.
Implement logic to respond appropriately to the predicted intent and entities.
When you have finished testing, enter quit.
