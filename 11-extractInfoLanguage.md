# Extracción de información de texto con Language
Objetivos:
- Detectar idioma
- Extracción de frases clave
- Análisis de opinión
- Extraer entidades
- Extracción de entidades vinculadas

## Aprovisionamiento de un Recurso Language

## Analisis de opinion
Si todos los resultados son neutro y una positiva, la valoración/opinión es positive. Si neutro/negativo = negativo.

## Extracción de entidades
### Linked entities
Si se detecta una entidad vinculada a una base de datos/link de información como wikipedia, se proporciona.

Si una entidad es **ambigua**, se utiliza la base knowledge de wikipedia con la desambiguación disponible.


# Laboratorio
Apunte importante:
 In this example, each review is analyzed individually, resulting in a separate call to the service for each file. An alternative approach is to create a collection of documents and pass them to the service in a single call. In both approaches, the response from the service consists of a collection of documents; which is why in the Python code above, the index of the first (and only) document in the response ([0]) is specified.

 ## Instrucciones:

 Prepare to use the Language SDK for text analytics
In this exercise, you'll complete a partially implemented client application that uses the Language service text analytics SDK to analyze hotel reviews.

Note: You can choose to use the SDK for either C# or Python. In the steps below, perform the actions appropriate for your preferred language.

In Visual Studio Code, in the Explorer pane, browse to the 05-analyze-text folder and expand the C-Sharp or Python folder depending on your language preference.

Right-click the text-analysis folder and open an integrated terminal. Then install the Text Analytics SDK package by running the appropriate command for your language preference:

C#

dotnet add package Azure.AI.TextAnalytics --version 5.3.0
Python

pip install azure-ai-textanalytics==5.3.0
View the contents of the text-analysis folder, and note that it contains a file for configuration settings:

C#: appsettings.json
Python: .env
Open the configuration file and update the configuration values it contains to reflect the endpoint and an authentication key for your cognitive services resource. Save your changes.

Note that the text-analysis folder contains a code file for the client application:

C#: Program.cs
Python: text-analysis.py
Open the code file and at the top, under the existing namespace references, find the comment Import namespaces. Then, under this comment, add the following language-specific code to import the namespaces you will need to use the Text Analytics SDK:

C#

C#
// import namespaces
using Azure;
using Azure.AI.TextAnalytics;
Python

Python
# import namespaces
from azure.core.credentials import AzureKeyCredential
from azure.ai.textanalytics import TextAnalyticsClient
In the Main function, note that code to load the cognitive services endpoint and key from the configuration file has already been provided. Then find the comment Create client using endpoint and key, and add the following code to create a client for the Text Analysis API:

C#

C#
// Create client using endpoint and key
AzureKeyCredential credentials = new AzureKeyCredential(cogSvcKey);
Uri endpoint = new Uri(cogSvcEndpoint);
TextAnalyticsClient CogClient = new TextAnalyticsClient(endpoint, credentials);
Python

Python
# Create client using endpoint and key
credential = AzureKeyCredential(cog_key)
cog_client = TextAnalyticsClient(endpoint=cog_endpoint, credential=credential)
Save your changes and return to the integrated terminal for the text-analysis folder, and enter the following command to run the program:

C#

dotnet run
Python

python text-analysis.py
Observe the output as the code should run without error, displaying the contents of each review text file in the reviews folder. The application successfully creates a client for the Text Analytics API but doesn't make use of it. We'll fix that in the next procedure.

Detect language
Now that you have created a client for the Text Analytics API, let's use it to detect the language in which each review is written.

In the Main function for your program, find the comment Get language. Then, under this comment, add the code necessary to detect the language in each review document:

C#

C
// Get language
DetectedLanguage detectedLanguage = CogClient.DetectLanguage(text);
Console.WriteLine($"\nLanguage: {detectedLanguage.Name}");
Python

Python
# Get language
detectedLanguage = cog_client.detect_language(documents=[text])[0]
print('\nLanguage: {}'.format(detectedLanguage.primary_language.name))
Note: In this example, each review is analyzed individually, resulting in a separate call to the service for each file. An alternative approach is to create a collection of documents and pass them to the service in a single call. In both approaches, the response from the service consists of a collection of documents; which is why in the Python code above, the index of the first (and only) document in the response ([0]) is specified.

Save your changes and return to the integrated terminal for the text-analysis folder, and enter the following command to run the program:

C#

dotnet run
Python

python text-analysis.py
Observe the output, noting that this time the language for each review is identified.

Evaluate sentiment
Sentiment analysis is a commonly used technique to classify text as positive or negative (or possible neutral or mixed). It's commonly used to analyze social media posts, product reviews, and other items where the sentiment of the text may provide useful insights.

In the Main function for your program, find the comment Get sentiment. Then, under this comment, add the code necessary to detect the sentiment of each review document:

C#

C
// Get sentiment
DocumentSentiment sentimentAnalysis = CogClient.AnalyzeSentiment(text);
Console.WriteLine($"\nSentiment: {sentimentAnalysis.Sentiment}");
Python

Python
# Get sentiment
sentimentAnalysis = cog_client.analyze_sentiment(documents=[text])[0]
print("\nSentiment: {}".format(sentimentAnalysis.sentiment))
Save your changes and return to the integrated terminal for the text-analysis folder, and enter the following command to run the program:

C#

dotnet run
Python

python text-analysis.py
Observe the output, noting that the sentiment of the reviews is detected.

Identify key phrases
It can be useful to identify key phrases in a body of text to help determine the main topics that it discusses.

In the Main function for your program, find the comment Get key phrases. Then, under this comment, add the code necessary to detect the key phrases in each review document:

C#

C
// Get key phrases
KeyPhraseCollection phrases = CogClient.ExtractKeyPhrases(text);
if (phrases.Count > 0)
{
    Console.WriteLine("\nKey Phrases:");
    foreach(string phrase in phrases)
    {
        Console.WriteLine($"\t{phrase}");
    }
}
Python

Python
# Get key phrases
phrases = cog_client.extract_key_phrases(documents=[text])[0].key_phrases
if len(phrases) > 0:
    print("\nKey Phrases:")
    for phrase in phrases:
        print('\t{}'.format(phrase))
Save your changes and return to the integrated terminal for the text-analysis folder, and enter the following command to run the program:

C#

dotnet run
Python

python text-analysis.py
Observe the output, noting that each document contains key phrases that give some insights into what the review is about.

Extract entities
Often, documents or other bodies of text mention people, places, time periods, or other entities. The text Analytics API can detect multiple categories (and subcategories) of entity in your text.

In the Main function for your program, find the comment Get entities. Then, under this comment, add the code necessary to identify entities that are mentioned in each review:

C#

C
// Get entities
CategorizedEntityCollection entities = CogClient.RecognizeEntities(text);
if (entities.Count > 0)
{
    Console.WriteLine("\nEntities:");
    foreach(CategorizedEntity entity in entities)
    {
        Console.WriteLine($"\t{entity.Text} ({entity.Category})");
    }
}
Python

Python
# Get entities
entities = cog_client.recognize_entities(documents=[text])[0].entities
if len(entities) > 0:
    print("\nEntities")
    for entity in entities:
        print('\t{} ({})'.format(entity.text, entity.category))
Save your changes and return to the integrated terminal for the text-analysis folder, and enter the following command to run the program:

C#

dotnet run
Python

python text-analysis.py
Observe the output, noting the entities that have been detected in the text.

Extract linked entities
In addition to categorized entities, the Text Analytics API can detect entities for which there are known links to data sources, such as Wikipedia.

In the Main function for your program, find the comment Get linked entities. Then, under this comment, add the code necessary to identify linked entities that are mentioned in each review:

C#

C
// Get linked entities
LinkedEntityCollection linkedEntities = CogClient.RecognizeLinkedEntities(text);
if (linkedEntities.Count > 0)
{
    Console.WriteLine("\nLinks:");
    foreach(LinkedEntity linkedEntity in linkedEntities)
    {
        Console.WriteLine($"\t{linkedEntity.Name} ({linkedEntity.Url})");
    }
}
Python

Python
# Get linked entities
entities = cog_client.recognize_linked_entities(documents=[text])[0].entities
if len(entities) > 0:
    print("\nLinks")
    for linked_entity in entities:
        print('\t{} ({})'.format(linked_entity.name, linked_entity.url))
Save your changes and return to the integrated terminal for the text-analysis folder, and enter the following command to run the program:

C#

dotnet run
Python

python text-analysis.py
Observe the output, noting the linked entities that are identified.