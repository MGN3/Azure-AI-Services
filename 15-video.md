# Analisis de video con Azure Video Indexer y Azure Media Services.
Objetivos:
- Describir las funcionalidades de Video Analyzer for Media
- Extraer información personalizada
- Usar los widgets y API de Video Analyzer for Media

## Links

	1- Azure media services:
	https://learn.microsoft.com/es-es/azure/media-services/
	2- Web Video Indexer:
	https://vi.microsoft.com/es-ES
	3- Portal Azure AI Video Indexer:
	https://www.videoindexer.ai/media/library
  4- Documentación Inglés Video Indexer:
  https://learn.microsoft.com/en-us/azure/azure-video-indexer/

## Herramientas disponibles
- Video Analyzer service(limitado y **deprecated**)
- Azure Video Indexer(sustituto Analyzer)
- Azure Media Services(usando suscripción Azure para no tener limites).
- Portal propio: https://www.videoindexer.ai/media/library
Tanto Indexer como Media pueden aprovisionarse en Azure.

## Funcionalidades
La principal es **extraer información** para analizarla o ayudar al **indexado** para búsquedas.

Pueden agruparse por:

	1- Video(caras, celebridades, ropa, fotogramas en negro...)
	2- Audio(detección aplausos, risas..., translación, transcripción...)
	3- Ambas(Palabras clave por escena, detección emoción...)
	4- Otras características(edición en linea, recomendaciones...)

Funcionalidades destacables(hay muchas más)
- Identificación de caras
- Reconocimeinto de texto y caracteres
- Etiquetado de objetos y elementos
- Scene Segmentations(separación del video por escenas diferenciadas?)
- Transcripción de speech
- Extracción key topics/temas
- Sentiment(positivo/negativo/neutral)
- Moderación de contenido
- Otras funcionalidades incluidas en Analisis de Imagen/Computer Vision

# Extacción de información personalizada
Al igual que con otros servicios de AI se puede configurar o entrenar el modelo para la detección/extracción de datos que nosotros queremos.
Ejemplos donde personalizar:

- Personas/people (Limited Access): Cara de personas que queremos reconocer, entrenando el modelo para que Video Indexer las reconozca en los vídeos.
- Lenguaje/Language: Entrenar el custom model para detectar y transcribir vocabulario/expresiones/lenguaje no común para detectarlo y poder transcribirlo/traducirlo etc...
- Marcas/brands: Detección de nombres, marcas, productos, proyectos, compañías de interés para la aplicación a crear.
- Elementos animados no humanos/animated characters

# Uso 
## Video Analyzer for Media widgets(aplicable a Video Indexer?)
The widgets used in the Video Analyzer for Media portal to play, analyze, and edit videos can be embedded in your own custom HTML interfaces. You can use this technique to share insights from specific videos with others without giving them full access to your account in the Video Analyzer for Media portal.

For example, a call to the https://api.videoindexer.ai.your_endpoint/Videos REST interface returns details of videos in your account, similar to the following JSON example:

JSON
```
{
  "results": [
    {
      "accountId":  "1234abcd-9876fghi-0156kihb-00123",
      "id":  "a12345bc6",
      "name":  "Responsible AI",
      "description":  "Microsoft Responsible AI video",
      "created":  "2021-01-05T15:33:58.918+00:00",
      "lastModified":  "2021-01-05T15:50:03.123+00:00",
      "lastIndexed":  "2021-01-05T15:34:08.007+00:00",
      "processingProgress":  "100%",
      "durationInSeconds":  114,
      "sourceLanguage":  "en-US",
    }
  ],
}
```

# Laboratorio
Link del video usado: https://aka.ms/responsible-ai-video

## Pasos:
- Subir el video
- Utilizar los gadgets haciendo click en "< embed >"
- Seleccionar el reproductor y utilizar un tamaño adecuado. Copiar el html.
- Seleccionar insights/información. Copiar el html.
- Utilizar el id de la cuenta Video Indexer: https://www.videoindexer.ai/account/779c2a22-9c4a-4c4e-9507-ce5609d1ff0b/settings
- Utilizar keys de Video Indexer disponibles en "profile"(url específica API): https://api-portal.videoindexer.ai/


## Codigo para incluir los widgets de Video Indexer.
```
<table>        <tr>            <td style="vertical-align:top;">                <!--Player widget goes here -->                <iframe width="560" height="315"                    src="https://www.videoindexer.ai/embed/player/779c2a22-9c4a-4c4e-9507-ce5609d1ff0b/0f28595b53/?accessToken=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJWZXJzaW9uIjoiMi4wLjAuMCIsIktleVZlcnNpb24iOiJlMzEzZDcxYmJmYTM0YzQyYjM2NDg5ZWM3NDczMTNmMSIsIkFjY291bnRJZCI6Ijc3OWMyYTIyLTljNGEtNGM0ZS05NTA3LWNlNTYwOWQxZmYwYiIsIkFjY291bnRUeXBlIjoiVHJpYWwiLCJWaWRlb0lkIjoiMGYyODU5NWI1MyIsIlBlcm1pc3Npb24iOiJSZWFkZXIiLCJFeHRlcm5hbFVzZXJJZCI6IjE6bGl2ZS5jb206MDAwNjAwMDBFRjcwMUE2OCIsIlVzZXJUeXBlIjoiTWljcm9zb2Z0Q29ycEFhZCIsIklzc3VlckxvY2F0aW9uIjoiVHJpYWwiLCJuYmYiOjE2OTEzMjQxNTEsImV4cCI6MTY5MTMyODA1MSwiaXNzIjoiaHR0cHM6Ly9hcGkudmlkZW9pbmRleGVyLmFpLyIsImF1ZCI6Imh0dHBzOi8vYXBpLnZpZGVvaW5kZXhlci5haS8ifQ.g6TO48tyWG5FPhqRtox21yPbUK0lm4jYxI5lX3bXwolPljSCoLyICZklfehvj44my7NsUsMjtkHpJ-zW6iEWbkmurVy3H_gi3-7XmGXvAvTzP1nQhTyMKKGCCrYUKwK7FnbmHUE5UEFlqEVNE4l_9MaM4KQEz0hZX1xmQnQuKqMTmHBNBAu2_-0mKRtix2frsVcxe3zP5GB9FKFJZSQ2_OXE5tkXgQd5oOGQLhgJiGFbEiYYjcjio1yP2fZX1fY0DDmuclQABVZgmQIq25YC_ZqVADu043TN3lF_uGlkE-xDNwed2MmpMtBzUYcLdjLWlldBFBAte-JM-Z8nveHFvQ&locale=es&location=trial"                    frameborder="1" allowfullscreen>                </iframe>            </td>            <td style="vertical-align:top;">                <!-- Insights widget goes here -->                <iframe width="580" height="780"                    src="https://www.videoindexer.ai/embed/insights/779c2a22-9c4a-4c4e-9507-ce5609d1ff0b/0f28595b53/?accessToken=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJWZXJzaW9uIjoiMi4wLjAuMCIsIktleVZlcnNpb24iOiJlMzEzZDcxYmJmYTM0YzQyYjM2NDg5ZWM3NDczMTNmMSIsIkFjY291bnRJZCI6Ijc3OWMyYTIyLTljNGEtNGM0ZS05NTA3LWNlNTYwOWQxZmYwYiIsIkFjY291bnRUeXBlIjoiVHJpYWwiLCJWaWRlb0lkIjoiMGYyODU5NWI1MyIsIlBlcm1pc3Npb24iOiJSZWFkZXIiLCJFeHRlcm5hbFVzZXJJZCI6IjE6bGl2ZS5jb206MDAwNjAwMDBFRjcwMUE2OCIsIlVzZXJUeXBlIjoiTWljcm9zb2Z0Q29ycEFhZCIsIklzc3VlckxvY2F0aW9uIjoiVHJpYWwiLCJuYmYiOjE2OTEzMjQxNTEsImV4cCI6MTY5MTMyODA1MSwiaXNzIjoiaHR0cHM6Ly9hcGkudmlkZW9pbmRleGVyLmFpLyIsImF1ZCI6Imh0dHBzOi8vYXBpLnZpZGVvaW5kZXhlci5haS8ifQ.g6TO48tyWG5FPhqRtox21yPbUK0lm4jYxI5lX3bXwolPljSCoLyICZklfehvj44my7NsUsMjtkHpJ-zW6iEWbkmurVy3H_gi3-7XmGXvAvTzP1nQhTyMKKGCCrYUKwK7FnbmHUE5UEFlqEVNE4l_9MaM4KQEz0hZX1xmQnQuKqMTmHBNBAu2_-0mKRtix2frsVcxe3zP5GB9FKFJZSQ2_OXE5tkXgQd5oOGQLhgJiGFbEiYYjcjio1yP2fZX1fY0DDmuclQABVZgmQIq25YC_ZqVADu043TN3lF_uGlkE-xDNwed2MmpMtBzUYcLdjLWlldBFBAte-JM-Z8nveHFvQ&locale=es&location=trial"                    frameborder="2" allowfullscreen>                </iframe>            </td>https://api-portal.videoindexer.ai    </table>
```


## Codigo para utilizar la api a través de powershell
```
$account_id="YOUR_ACCOUNT_ID"$api_key="YOUR_API_KEY"$location="trial"# Call the AccessToken method with the API key in the header to get an access token$token = Invoke-RestMethod -Method "Get" -Uri "https://api.videoindexer.ai/auth/$location/Accounts/$account_id/AccessToken" -Headers @{'Ocp-Apim-Subscription-Key' = $api_key}# Use the access token to make an authenticated call to the Videos method to get a list of videos in the accountInvoke-RestMethod -Method "Get" -Uri "https://api.videoindexer.ai/$location/Accounts/$account_id/Videos?accessToken=$token" | ConvertTo-Json
```

////////////////////
### TEXTO DE LA PRÁCTICA
Upload a video to Video Analyzer
First, you'll need to sign into the Video Analyzer portal and upload a video.

Tip: If the Video Analyzer page is slow to load in the hosted lab environment, use your locally installed browser. You can switch back to the hosted VM for the later tasks.

In your browser, open the Video Analyzer portal at https://www.videoindexer.ai.
If you have an existing Video Analyzer account, sign in. Otherwise, sign up for a free account and sign in using your Microsoft account (or any other valid account type). If you have difficulty signing in, try opening a private browser session.
In Video Analyzer, select the Upload option. Then select the option to enter a file URL and enter https://aka.ms/responsible-ai-video. Change the default name to Responsible AI, review the default settings, select the checkbox to verify compliance with Microsoft's policies for facial recognition, and upload the file.
After the file has uploaded, wait a few minutes while Video Analyzer automatically indexes it.
Note: In this exercise, we're using this video to explore Video Analyzer functionality; but you should take the time to watch it in full when you've finished the exercise as it contains useful information and guidance for developing AI-enabled applications responsibly!

Review video insights
The indexing process extracts insights from the video, which you can view in the portal.

In the Video Analyzer portal, when the video is indexed, select it to view it. You'll see the video player alongside a pane that shows insights extracted from the video.
Video Analyzer with a video player and Insights pane

As the video plays, select the Timeline tab to view a transcript of the video audio.
Video Analyzer with a video player and Timeline pane showing the video transcript.

At the top right of the portal, select the View symbol (which looks similar to 🗇), and in the list of insights, in addition to Transcript, select OCR and Speakers.
Video Analyzer view menu with Transcript, OCR, and Speakers selected

Observe that the Timeline pane now includes:

Transcript of audio narration.
Text visible in the video.
Indications of speakers who appear in the video. Some well-known people are automatically recognized by name, others are indicated by number (for example Speaker #1).
Switch back to the Insights pane and view the insights show there. They include:

Individual people who appear in the video.
Topics discussed in the video.
Labels for objects that appear in the video.
Named entities, such as people and brands that appear in the video.
Key scenes.
With the Insights pane visible, select the View symbol again, and in the list of insights, add Keywords and Sentiments to the pane.

The insights found can help you determine the main themes in the video. For example, the topics for this video show that it is clearly about technology, social responsibility, and ethics.

Search for insights
You can use Video Analyzer to search the video for insights.

In the Insights pane, in the Search box, enter Bee. You may need to scroll down in the Insights pane to see results for all types of insight.
Observe that one matching label is found, with its location in the video indicated beneath.
Select the beginning of the section where the presence of a bee is indicated, and view the video at that point (you may need to pause the video and select carefully - the bee only appears briefly!)
Clear the Search box to show all insights for the video.
Video Analyzer search results for Bee

Use Video Analyzer widgets
The Video Analyzer portal is a useful interface to manage video indexing projects. However, there may be occasions when you want to make the video and its insights available to people who don't have access to your Video Analyzer account. Video Analyzer provides widgets that you can embed in a web page for this purpose.

In Visual Studio Code, in the 16-video-indexer folder, open analyze-video.html. This is a basic HTML page to which you will add the Video Analyzer Player and Insights widgets. Note the reference to the vb.widgets.mediator.js script in the header - this script enables multiple Video Analyzer widgets on the page to interact with one another.
In the Video Analyzer portal, return to the Media files page and open your Responsible AI video.
Under the video player, select </> Embed to view the HTML iframe code to embed the widgets.
In the Share and Embed dialog box, select the Player widget, set the video size to 560 x 315, and then copy the embed code to the clipboard.
In Visual Studio Code, in the analyze-video.html file, paste the copied code under the comment <-- Player widget goes here -- >.
Back in the Share and Embed dialog box, select the Insights widget and then copy the embed code to the clipboard. Then close the Share and Embed dialog box, switch back to Visual Studio Code, and paste the copied code under the comment <-- Insights widget goes here -- >.
Save the file. Then in the Explorer pane, right-click analyze-video.html and select Reveal in File Explorer.
In File Explorer, open analyze-video.html in your browser to see the web page.
Experiment with the widgets, using the Insights widget to search for insights and jump to them in the video.
Video Analyzer widgets in a web page

Use the Video Analyzer REST API
Video Analyzer provides a REST API that you can use to upload and manage videos in your account.

Get your API details
To use the Video Analyzer API, you need some information to authenticate requests:

In the Video Analyzer portal, expand the menu (≡) and select the Account settings page.
Note the Account ID on this page - you will need it later.
Open a new browser tab and go to the Video Analyzer developer portal at https://api-portal.videoindexer.ai, signing in using the credentials for your Video Analyzer account.
On the Profile page, view the Subscriptions associated with your profile.
On the page with your subscription(s), observe that you have been assigned two keys (primary and secondary) for each subscription. Then select Show for any of the keys to see it. You will need this key shortly.
Use the REST API
Now that you have the account ID and an API key, you can use the REST API to work with videos in your account. In this procedure, you'll use a PowerShell script to make REST calls; but the same principles apply with HTTP utilities such as cURL or Postman, or any programming language capable of sending and receiving JSON over HTTP.

All interactions with the Video Analyzer REST API follow the same pattern:

An initial request to the AccessToken method with the API key in the header is used to obtain an access token.
Subsequent requests use the access token to authenticate when calling REST methods to work with videos.
In Visual Studio Code, in the 16-video-indexer folder, open get-videos.ps1.
In the PowerShell script, replace the YOUR_ACCOUNT_ID and YOUR_API_KEY placeholders with the account ID and API key values you identified previously.
Observe that the location for a free account is "trial". If you have created an unrestricted Video Analyzer account (with an associated Azure resource), you can change this to the location where your Azure resource is provisioned (for example "eastus").
Review the code in the script, noting that invokes two REST methods: one to get an access token, and another to list the videos in your account.
Save your changes, and then at the top-right of the script pane, use the ▷ button to run the script.
View the JSON response from the REST service, which should contain details of the Responsible AI video you indexed previously.