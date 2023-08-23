# Cognitive Service Vision
Link práctica: https://microsoftlearning.github.io/mslearn-cognitive-vision/instructions/05-exercise-explore-image-analysis.html
Link informativo/práctico Vision Studio:
https://www.youtube.com/watch?v=4shB_qdU3Gs

La tecnología base se llama **Florence**.

Objetivos:
- Describa las funcionalidades disponibles en Azure Cognitive Service para visión.
- Comprenda las ventajas proporcionadas por el modelo base Florence de Microsoft.
- Análisis de imágenes mediante Image Analysis 4.0.
- Use Vision Studio para crear un modelo de detección de objetos personalizado.

### Posibles alternativas a CSVision
PRINCIPAL POSIBLE ALTERNARTIVA: Meta Segmentation: **SAM**.

MLflow & MLKit(Databricks), CUDA(NVIDIA), H2O, NLTK, PredictionIO, XGBoost y Theano1.

## Funcionalidades
- Subtitular imágenes
- Descripción y generación de etiquetas
- Caption y Caption extra(se llamada de otra forma, no extra).
- Descripcion de imágenes(personas ciegas? Netflix y otras tendrán servicios como este?)
- Extracción y lectura de texto.
- Detección de caras
- Detección de objetos
- Detección de marcas, logotipos...
- Eliminación de background.
- Detección de anomalías
- Generación automatica de descripciones de imágenes y sus partes
- Moderar contenido
- Creación de thumbnails
- Modelos de visión personalizados.
- Análisis de metadatos, colores y tipos de img.
- UI Vision Studio para desarrolladores sin conocimientos profundos en ML
- Detección de objetos específicos con un modelo entrenado personalizado con asistencia de ML de Azure para entrenarlo.

# Florence model
Desarrollado para mejorar las capacidades de Vision Image Analysis.
Reconoce diferencias y similitudes entre imágenes, asociando imagenes similares con misma etiqueta(few-shot learning).

Antes de Florence, se requería un mínimo de 15 imágenes, ahora 4.

## Resumen Florence
Florence foundation model enables developers to create robust, market-ready computer vision applications capable of connecting their data to natural language interactions, unlocking powerful insights from their image and video content, and extracting information from visual features.

### Image retrieval
Uso de un mapa de vectores multi-dimensional donde poder realizar una búsqueda optima por proximidad de vectores con el input-imagen.

El input se convierte en vector y se compara con el mapa de vectores por cercanía semántica.

## CASO DE USO IMPORTANTE
Image Retrieval enables developers to build the capability to search images using natural language queries and get results based on image contents rather than having to rely on manually assigned labels or tags. NO METADATA OR TAGS NEEDED.

## Etiquetado de elementos
Funcionalidad básica donde se generane tags de elementos o conjuntos en la imagen.
- Contenido de la imagen.
- Interior o exterior
- Actividades
- Paisajes
- Lugares comunes/típicos(iglesia)
- Plantas
- Animales.
- Otros objetos y acciones.

## Detección de objetos en imágenes
**DIFERENTE A ETIQUETADO**
La diferencia principal es que se devuelve una TAG y la caja con las coordenadas de cada objeto. 

Esta función puede usarse para procesar relaciones entre objetos de un aimagen o detecctar varias repeticiones del mismo objeto en una imágen.

**Solo para objetos y entidades vivas**

## Lectura de texto con reconocimiento óptico de caracteres

Image Analysis 4.0 permite extraer texto manuscrito o impreso de imágenes.

### OCR
Optical Character Recognition es una "unified performance-enhanced synchronous API.
Esta API simplifica el proceso y tiene soporte de idiomas global.


### Creación de miniaturas con Smart Cropping.
**Smart Cropping API** para hacer thumbnails.

Cuando personas son detectadas en una imagen, las cajas de esas personas tienen prioridad para que creen un thumbnail.

Pueden usarse las coordenadas de la caja de objeto para usar el thumbnail.

### Detección de personas
Para cada persona detectada en una imagen se pueden devolver las coordenadas de la caja y un indice de precisión.

**NO** se diferencian caras o predicen atributos faciales.

### Background removal
Útil para procesar muchas imágenes y extraer solo los elementos/objetos presentes en ella.

# Vision Studio
Plataforma no-code/low-code para utilizar o configurar servicios de visión.

Try-it-out para practicar:

Optical character recognition:

## Extract text from images:
-Extract printed and handwritten text from images and documents

## Spatial analysis:

- Video summary and frame locator:
- Generate a summary of the main points shown in a video, locate specific keywords, and jump to the relevant section of the video
- Count people in an area: Analyze real-time video to count the number of people in a designated zone
- Detect when people cross a line: Analyze real-time streaming video to detect when a person crosses a line
- Detect when people enter/exit a zone: Analyze real-time streaming video to detect when a person enters or exits an area
- Monitor social distancing: Analyze real-time streaming video to determine if people violate a distance rule

## Image analysis:

- Search photos with natural language: Retrieve specific moments within a photo album based on vectorized text and image queries
- Add captions to images: Generate human-readable English sentences that describe the content of an image
- Dense captioning: Generate human-readable English phrases for all important objects within an image
- Detect common objects in images: Recognize objects of interested in an image and record their location using bounding boxes
- Extract common tags from images: Automatically assign one or more labels to an image based on its content
- Detect sensitive content in images: Moderate usage of images by detecting sensitive content


# Recursos:
- Resource group(grupo creado de recursos en AP)
- Cognitive Services/Computer Vision
- Azure Storage Account(Cuenta de Almacenamiento).
En Storage Account, creé tres contenedores con permisos blob.
Uno para la mayor parte de imágenes, otro para entrenamiento y otro analisis.

# Conclusiones
Servicio completo con muchas opciones y funcionalidades diferentes. 
Interesante para utilizar a nivel industrial o para detección avanzada en contextos de alta tecnología o IoT.

El proceso de entrenar con ML Azure puede tardar tiempo aunque se han reducido en muchos aspectos el tiempo necesario para tener un modelo entrenado funcional. Entrenar modelos personalizados requiere un poco más de conocimientos de Azure y sus recuros, como el uso de Storage Account y contenedores(blobs) dentro del mismo para agrupar diferentes tipos de imágenes.

En el proceso de entrenamiento no pudo realizarse alguna acción y luego no me permitió evaluar el modelo entrenado correctamente, aunque parecía funcionar correctamente.

Una vez etiquetado objetos en imágenes y que luego despues del ML se autoetiqueten y confirme su exactitud, es importante dedicar horas de reentreno para que sea más efectivo(horas sujetas a cobros).

# Funcionalidades Laboratorio
- Caption: Da una breve explicación de la imagen.
- Dense Caption: Da breves titulos a varios de los objetos/elementos presentes en la imagen.
- Extract Tags: Extrae etiquetas/nombres de elementos o conceptos inferidos de la imagen. (de pie, ropa, persona, tienda, venta, cliente...)
- Detect Common Object in Images: Detecta objetos. Se puede especificar que solo muestre los objetos a partir de un cierto grado de certidumbre.
- OCR Optical Character Recognition: Extrae texto de diferentes fuentes y estilos en imágenes.
- Image retrieval: Consiste en consultas en lenguaje natural y recibir las imágenes que coincidan con el input. 
**No** pude probarlo por disponibilidad solo US east.
Se puede acceder a un Azure Storage con imágenes y ahí realizar busquedas.
Para busquedas con muchas imágenes es nedcesario hacer otros pasos extra.
- Eliminación de Background: PENDIENTE USO DE VSCODE

# Laboratorio

## Custom Model/Modelo Personalizado
Link practica: https://microsoftlearning.github.io/mslearn-cognitive-vision/instructions/07-exercise-train-custom-object-detection-model.html

    1- Elegir funcionalidad(detect common objects in images)
    2- Crear Dataset en Vision Studio con las imágenes evaluation(incluye COCO file)
    3- Crear un Dataset en Vision Studio con las training images.
    4- Además, en el anterior dataset, es necesario crear un Azure ML data labeling proyect para entrenarlo con nuestras imágenes.(Create new Workspace. Necesario esperar un poco para que aparezca).
    5- Go to Custom_Object_Detection_labeling(o nombre dado en Vision Studio)
    6- Add label categories. Añadir los nombres o etiquetas que vamos a necesitar(pueden hacerse subetiquetas(tipos de manzana)).
    7- ML assisted labeling: activar
    Enable ML assisted labeling y también
    Attempt pre-labeling of manual tasks
    8- ESPERAR A:
    MLAssist re-inference run completed for labeling project "Custom"
    21 new prelabeled tasks were generated.
    Job details
    August 4, 2023 1:15 PM
    New MLAssist re-inference run started for labeling project "Custom"
    A new inference model was generated. Now attempting to convert manual tasks into prelabeled tasks…
    Job details
    August 4, 2023 1:14 PM
    9- De nuevo, Label Data-> Revisar las label automatizadas para corregir o añadir mejores labels.
    10- Una vez Importamos el resultado de ML labeling(El archivo COCO) en el dataset con el que hemos creado el workshop ML, vamos a Custom models y train a new model en Vision Studio.
    11- Ahora hay que seleccionar el training dataset y evaluation dataset(opcional) y que ambos se correspondan con los dataset adecuados.
    12- Escoger número de horas de entrenamiento



# TEXTO ELIMINACIÓN DE BACKGROUND
## NO pude hacer esta parte porque solo hay acceso desde us east.
The Background removal feature of Image Analysis 4.0 is useful when extracting information and content from images with distracting or noisy backgrounds. You need to focus your analysis on the most critical visual features. Background removal is unavailable as a try-it-out experience in Vision Studio, so you will access it via the Segment API endpoint.

For this task, you will use the REST Client extension of Visual Studio Code to make calls to the endpoint.

Download Visual Studio Code if you don’t already have it installed, and then start Visual Studio Code.

In Visual Studio Code, select the Extensions icon in the left-hand toolbar, enter “REST Client” into the Search Extensions in Marketplace box, and then select Install for the REST Client extension.

The Visual Studio Code extensions tab is displayed, with REST Client highlighted in the search bar and the REST Client extension highlighted in the search results. The Install button for the extension is highlighted.

Download the image-analysis.http file from here: https://github.com/MicrosoftLearning/mslearn-cognitive-vision/raw/main/assets/image-analysis.http

Note: If the file opens in the browser window, copy its raw text and paste it into a new file in Visual Studio Code named image-analysis.http. Alternatively, you can right-click on the page in the browser, select Save As, and then download the file that way. This method may require manually renaming the file to remove the .txt extension.

Open the image-analysis.http file using Visual Studio Code.

Before you can run the first command, you need to retrieve the subscription key for your Cognitive Services account from the Azure portal. In a browser window, open the Azure portal, and navigate to the cog-ms-learn-vision-SUFFIX Cognitive Service resource you created above.

On the Cognitive Services page, select Keys and Endpoint under Resource Management from the left-hand navigation menu. Copy the KEY 1 value by selecting the Copy to clipboard icon to the right of the key value.

The Cognitive Services page is displayed in the Azure portal. In the left-hand navigation menu, Keys and Endpoint is highlighted and selected. The copy to clipboard button is highlighted next to the KEY 1 value.

Return to the image-analysis.http file in Visual Studio code and replace [INSERT YOUR COGNITIVE SERVICES SUBSCRIPTION KEY] with the KEY 1 value you copied.

[INSERT YOUR COGNITIVE SERVICES SUBSCRIPTION KEY] is highlighted after the @subscriptionKey variable.

With the key updated, you are now ready to send requests to the Segment API to perform the background removal steps.

To see the impact of background removal, you will use the image below:

Image of a woman and girl taking a photo with a phone in a grocery store.

Before removing the image background, you will call the Image Analysis Analyze API to generate a caption for the original image. In the image-analysis.http file, select Send Request below the ### Generate a caption for the original image header.

The Send Request link is highlighted in the image-analysis.http file under the Generate a caption for the original image header.

Note the caption returned in the Response tab that opens:

Code
 {
   "captionResult": {
     "text": "a woman and a girl in a grocery store",
     "confidence": 0.4891723394393921
   },
   "modelVersion": "2023-02-01-preview",
   "metadata": {
     "width": 1024,
     "height": 682
   }
 }
The caption should be similar to “a woman and a girl in a grocery store.” While this caption is accurate, it does not necessarily give us any context into what the people in the photo are doing. The image captioning process evaluates all of the available information in the image to create the caption, resulting in some crucial details potentially being left out of the caption.

Return to the image-analysis.http file and select Send Request below the ### Background removal header to submit an HTTP request to the Segment API to perform background removal on the image specified in the url parameter in the body of the request.

The Send Request link is highlighted in the image-analysis.http file under the Background removal header.

After a few seconds, the response from the API will be returned in a new tab in Visual Studio Code. The response should look similar to the following:

The image above with the woman and girl taking a photo in a grocery store is displayed with the background removed.

Now, you will generate a caption for the image with the background removed. In the image-analysis.http file, select Send Request below the ### Generate a caption for the image with the background removed header.

The Send Request link is highlighted in the image-analysis.http file under the Generate a caption for the image with the background removed header.

Note the caption returned in the Response tab that opens and compare it to the caption that was generated for the original image:

Code
{
  "captionResult": {
    "text": "a woman and a girl taking a picture",
    "confidence": 0.39474931359291077
  },
  "modelVersion": "2023-02-01-preview",
  "metadata": {
    "width": 1024,
    "height": 682
  }
}
The caption should be, “a woman and a girl taking a picture.” Using the Background removal feature of Image Analysis 4.0, you can reduce the information in the photo down to the main subjects, eliminating any background noise from the picture. By removing the background, Image Analysis can identify different details as the focus, providing better context into what the photo’s subjects are doing.

Congratulations! You have successfully used Vision Studio to try out the new features of Image Analysis 4.0.