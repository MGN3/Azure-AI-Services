# Detección, análisis y reconocimiento de caras
Objetivos:
- Identificar las opciones para el análisis de detección de caras
- Describir las consideraciones para el análisis de caras
- Detectar caras con el servicio Computer Vision
- Describir las funcionalidades del servicio Face
- Obtenga información sobre el reconocimiento facial.

Limitación principal para **Face AP
https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-limited-access

# Recursos
	
	1- Computer Vision(ubicación cara)
	El servicio Computer Vision permite detectar caras humanas en una imagen y devolver un rectángulo delimitador para su ubicación.

	2- Face(muchas funcionalidades)
		- Ubicación
		- Atributos
		- Posición cabeza
		- Verificación
		- Reconocimiento
	
# Privacidad y seguridad
- **Seguridad y privacidad de los datos**. Los datos faciales son de identificación personal y deben considerarse confidenciales y privados. Debe asegurarse de que ha implementado la protección adecuada de los datos faciales usados para el entrenamiento y la inferencia de modelos.
- **Transparencia**. Asegúrese de que los usuarios estén informados sobre cómo se usarán sus datos faciales y quién tendrá acceso a ellos.
- **Equidad e inclusión**. Asegúrese de que el sistema basado en caras no se puede usar de una manera que sea perjudicial para las personas en función de su apariencia, o para dirigirse de forma injusta a las personas.

# Computer Vision para caras
Docu oficial:
https://learn.microsoft.com/es-es/rest/api/computer-vision/

Antes, había predicción de genero y edad. Eliminado por temas de seguridad y uso responsable.

La función **AnalyzeFaces(imgFile)** de REST si especificas **Faces** como parámetro devuelve el JSON con los datos del recuadro/coordenadas de cada cara detectada.

```
{
  "faces": [
      {
        "faceRectangle": {
          "top": 225,
          "left": 237,
          "width": 130,
          "height": 130
        }
      },
      {
        "faceRectangle": {
          "top": 309,
          "left": 534,
          "width": 119,
          "height": 119
      }
    }
  ]
}
```

# Face service
Se puede usar como single-service (Face API). O como multi-service con Cognitive Services resource.

Funcionalidades:

- Face detection - for each detected face, the results include an ID that identifies the face and the bounding box coordinates indicating its location in the image.

- Face attribute analysis - you can return a wide range of facial attributes, including:

	- Head pose (pitch, roll, and yaw orientation in 3D space)
	- Glasses (NoGlasses ,ReadingGlasses, Sunglasses, or Swimming Goggles)
	- Blur (low, medium, or high)
	- Exposure (underExposure, goodExposure, or overExposure)
	- Noise (visual noise in the image)
	- Occlusion (objects obscuring the face)

- Facial landmark location - coordinates for key landmarks in relation to facial features (for example, eye corners, pupils, tip of nose, and so on)

- Face comparison - you can compare faces across multiple images for similarity (to find individuals with similar facial features) and **verification** (to determine that a face in one image is the same person as a face in another image)

- Facial recognition - you can train a model with a collection of faces belonging to specific individuals, and use the model to identify those people in new images.

![esquemaFace](https://learn.microsoft.com/en-us/training/wwl-data-ai/detect-analyze-recognize-faces/media/face-service.png)

## Comparación y verificación de caras detectadas.

Al detectar una cara se asigna un ID a la misma(GUID, anónimo) dsurante 24 horas. Otras imágenes con caras y IDs se pueden comparar con este ID inicial...

Compara caras de forma anónima mediante la asignación de un ID a cada cara detectada puede ser útil en sistemas en los que es importante confirmar que la misma persona está presente en dos ocasiones, sin necesidad de conocer la identidad real de la persona. 
Por ejemplo, tomando imágenes de personas que entran y salen de un espacio seguro para verificar que todos los que entraron salen.

![esquemaFace](https://learn.microsoft.com/en-us/training/wwl-data-ai/detect-analyze-recognize-faces/media/face-matching.png)

## Implementación de reconocimiento facial

Se puede entrenar un modelo de reconocimiento facial.
	
	1- Crear grupo(empleados)
	2- Añadir persona
	3- Añadir diferentes imágenes de la misma persona en diferentes poses(estos id de caras no se eliminan a las 24 horas. Se denominan *persisted faces*)
	4- Entrenamiento

![esquemaFace](https://learn.microsoft.com/en-us/training/wwl-data-ai/detect-analyze-recognize-faces/media/person-groups.png)

Este modelo se almacena en el recurso utilizado(Face o Cognitive S.) y puede consumirse para:
- Identificar individuos en imágenes
- Verificar la identidad de una cara
- Analizar nuevas imágenes y ver similitud con otras *persisted faces*.

# Preguntas
1-Which of the following facial attributes can the Computer Vision service predict? 

Location.
That's correct. The Computer Vision service predicts location for a detected face.


Type of eye-glasses.

Occlusion.
2-You need to create a facial recognition solution to identify named employees. Which service should you use? 

Computer Vision.

Custom Vision.

Face.
That's correct. Use the Face service to create a facial recognition solution.

3-You need to verify that the person in a photo taken at hospital reception is the same person in a photo taken at a ward entrance 10 minutes later. What should you do? 

Create a People Group and add a person for every hospital visitor with multiple photographs to train a model.

Verify the face in the ward photo by comparing it to the detected face ID from the reception photo.
That's correct. The most efficient approach is to compare the two faces using the detected face ID within 24 hours.


Compare the Age, head pose, and hair color for the faces in the reception and ward photo's.


# Laboratorio


## Computer Vision Cognitive Services

//////////////////////////////////
# Texto Laboratorio:

The ability to detect and analyze human faces is a core AI capability. In this exercise, you'll explore two Azure Cognitive Services that you can use to work with faces in images: the Computer Vision service, and the Face service.

Note: From June 21st 2022, capabilities of cognitive services that return personally identifiable information are restricted to customers who have been granted limited access. Additionally, capabilities that infer emotional state are no longer available. These restrictions may affect this lab exercise. We're working to address this, but in the meantime you may experience some errors when following the steps below; for which we apologize. For more details about the changes Microsoft has made, and why - see Responsible AI investments and safeguards for facial recognition.

Clone the repository for this course
If you have not already done so, you must clone the code repository for this course:

Start Visual Studio Code.

Open the palette (SHIFT+CTRL+P) and run a Git: Clone command to clone the https://github.com/MicrosoftLearning/AI-102-AIEngineer repository to a local folder (it doesn't matter which folder).

When the repository has been cloned, open the folder in Visual Studio Code.

Wait while additional files are installed to support the C# code projects in the repo.

Note: If you are prompted to add required assets to build and debug, select Not Now.

Provision a Cognitive Services resource
If you don't already have one in your subscription, you'll need to provision a Cognitive Services resource.

Open the Azure portal at https://portal.azure.com, and sign in using the Microsoft account associated with your Azure subscription.
Select the ＋Create a resource button, search for cognitive services, and create a Cognitive Services resource with the following settings:
Subscription: Your Azure subscription
Resource group: Choose or create a resource group (if you are using a restricted subscription, you may not have permission to create a new resource group - use the one provided)
Region: Choose any available region
Name: Enter a unique name
Pricing tier: Standard S0
Select the required checkboxes and create the resource.
Wait for deployment to complete, and then view the deployment details.
When the resource has been deployed, go to it and view its Keys and Endpoint page. You will need the endpoint and one of the keys from this page in the next procedure.
Prepare to use the Computer Vision SDK
In this exercise, you'll complete a partially implemented client application that uses the Computer Vision SDK to analyze faces in an image.

Note: You can choose to use the SDK for either C# or Python. In the steps below, perform the actions appropriate for your preferred language.

In Visual Studio Code, in the Explorer pane, browse to the 19-face folder and expand the C-Sharp or Python folder depending on your language preference.

Right-click the computer-vision folder and open an integrated terminal. Then install the Computer Vision SDK package by running the appropriate command for your language preference:

C#

dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 6.0.0
Python

pip install azure-cognitiveservices-vision-computervision==0.7.0
View the contents of the computer-vision folder, and note that it contains a file for configuration settings:

C#: appsettings.json
Python: .env
Open the configuration file and update the configuration values it contains to reflect the endpoint and an authentication key for your cognitive services resource. Save your changes.

Note that the computer-vision folder contains a code file for the client application:

C#: Program.cs
Python: detect-faces.py
Open the code file and at the top, under the existing namespace references, find the comment Import namespaces. Then, under this comment, add the following language-specific code to import the namespaces you will need to use the Computer Vision SDK:

C#

C#
// import namespaces
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
Python

Python
# import namespaces
from azure.cognitiveservices.vision.computervision import ComputerVisionClient
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials
View the image you will analyze
In this exercise, you will use the Computer Vision service to analyze an image of people.

In Visual Studio Code, expand the computer-vision folder and the images folder it contains.
Select the people.jpg image to view it.
Detect faces in an image
Now you're ready to use the SDK to call the Computer Vision service and detect faces in an image.

In the code file for your client application (Program.cs or detect-faces.py), in the Main function, note that the code to load the configuration settings has been provided. Then find the comment Authenticate Computer Vision client. Then, under this comment, add the following language-specific code to create and authenticate a Computer Vision client object:

C#

C#
// Authenticate Computer Vision client
ApiKeyServiceClientCredentials credentials = new ApiKeyServiceClientCredentials(cogSvcKey);
cvClient = new ComputerVisionClient(credentials)
{
    Endpoint = cogSvcEndpoint
};
Python

Python
# Authenticate Computer Vision client
credential = CognitiveServicesCredentials(cog_key) 
cv_client = ComputerVisionClient(cog_endpoint, credential)
In the Main function, under the code you just added, note that the code specifies the path to an image file and then passes the image path to a function named AnalyzeFaces. This function is not yet fully implemented.

In the AnalyzeFaces function, under the comment Specify features to be retrieved (faces), add the following code:

C#

C#
// Specify features to be retrieved (faces)
List<VisualFeatureTypes?> features = new List<VisualFeatureTypes?>()
{
    VisualFeatureTypes.Faces
};
Python

Python
# Specify features to be retrieved (faces)
features = [VisualFeatureTypes.faces]
In the AnalyzeFaces function, under the comment Get image analysis, add the following code:

C#

C
// Get image analysis
using (var imageData = File.OpenRead(imageFile))
{    
    var analysis = await cvClient.AnalyzeImageInStreamAsync(imageData, features);

    // Get faces
    if (analysis.Faces.Count > 0)
    {
        Console.WriteLine($"{analysis.Faces.Count} faces detected.");

        // Prepare image for drawing
        Image image = Image.FromFile(imageFile);
        Graphics graphics = Graphics.FromImage(image);
        Pen pen = new Pen(Color.LightGreen, 3);
        Font font = new Font("Arial", 3);
        SolidBrush brush = new SolidBrush(Color.LightGreen);

        // Draw and annotate each face
        foreach (var face in analysis.Faces)
        {
            var r = face.FaceRectangle;
            Rectangle rect = new Rectangle(r.Left, r.Top, r.Width, r.Height);
            graphics.DrawRectangle(pen, rect);
            string annotation = $"Person at approximately {r.Left}, {r.Top}";
            graphics.DrawString(annotation,font,brush,r.Left, r.Top);
        }

        // Save annotated image
        String output_file = "detected_faces.jpg";
        image.Save(output_file);
        Console.WriteLine(" Results saved in " + output_file);   
    }
}        
Python

Python
# Get image analysis
with open(image_file, mode="rb") as image_data:
    analysis = cv_client.analyze_image_in_stream(image_data , features)

    # Get faces
    if analysis.faces:
        print(len(analysis.faces), 'faces detected.')

        # Prepare image for drawing
        fig = plt.figure(figsize=(8, 6))
        plt.axis('off')
        image = Image.open(image_file)
        draw = ImageDraw.Draw(image)
        color = 'lightgreen'

        # Draw and annotate each face
        for face in analysis.faces:
            r = face.face_rectangle
            bounding_box = ((r.left, r.top), (r.left + r.width, r.top + r.height))
            draw = ImageDraw.Draw(image)
            draw.rectangle(bounding_box, outline=color, width=5)
            annotation = 'Person at approximately {}, {}'.format(r.left, r.top)
            plt.annotate(annotation,(r.left, r.top), backgroundcolor=color)

        # Save annotated image
        plt.imshow(image)
        outputfile = 'detected_faces.jpg'
        fig.savefig(outputfile)

        print('Results saved in', outputfile)
Save your changes and return to the integrated terminal for the computer-vision folder, and enter the following command to run the program:

C#

dotnet run
Python

python detect-faces.py
Observe the output, which should indicate the number of faces detected.

View the detected_faces.jpg file that is generated in the same folder as your code file to see the annotated faces. In this case, your code has used the attributes of the face to label the location of the top left of the box, and the bounding box coordinates to draw a rectangle around each face.

Prepare to use the Face SDK
While the Computer Vision service offers basic face detection (along with many other image analysis capabilities), the Face service provides more comprehensive functionality for facial analysis and recognition.

In Visual Studio Code, in the Explorer pane, browse to the 19-face folder and expand the C-Sharp or Python folder depending on your language preference.

Right-click the face-api folder and open an integrated terminal. Then install the Face SDK package by running the appropriate command for your language preference:

C#

dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.6.0-preview.1
Python

pip install azure-cognitiveservices-vision-face==0.4.1
View the contents of the face-api folder, and note that it contains a file for configuration settings:

C#: appsettings.json
Python: .env
Open the configuration file and update the configuration values it contains to reflect the endpoint and an authentication key for your cognitive services resource. Save your changes.

Note that the face-api folder contains a code file for the client application:

C#: Program.cs
Python: analyze-faces.py
Open the code file and at the top, under the existing namespace references, find the comment Import namespaces. Then, under this comment, add the following language-specific code to import the namespaces you will need to use the Computer Vision SDK:

C#

C#
// Import namespaces
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;
Python

Python
# Import namespaces
from azure.cognitiveservices.vision.face import FaceClient
from azure.cognitiveservices.vision.face.models import FaceAttributeType
from msrest.authentication import CognitiveServicesCredentials
In the Main function, note that the code to load the configuration settings has been provided. Then find the comment Authenticate Face client. Then, under this comment, add the following language-specific code to create and authenticate a FaceClient object:

C#

C#
// Authenticate Face client
ApiKeyServiceClientCredentials credentials = new ApiKeyServiceClientCredentials(cogSvcKey);
faceClient = new FaceClient(credentials)
{
    Endpoint = cogSvcEndpoint
};
Python

Python
# Authenticate Face client
credentials = CognitiveServicesCredentials(cog_key)
face_client = FaceClient(cog_endpoint, credentials)
In the Main function, under the code you just added, note that the code displays a menu that enables you to call functions in your code to explore the capabilities of the Face service. You will implement these functions in the remainder of this exercise.

Detect and analyze faces
One of the most fundamental capabilities of the Face service is to detect faces in an image, and determine their attributes, such as head pose, blur, the presence of spectacles, and so on.

In the code file for your application, in the Main function, examine the code that runs if the user selects menu option 1. This code calls the DetectFaces function, passing the path to an image file.

Find the DetectFaces function in the code file, and under the comment Specify facial features to be retrieved, add the following code:

C#

C#
// Specify facial features to be retrieved
List<FaceAttributeType?> features = new List<FaceAttributeType?>
{
    FaceAttributeType.Occlusion,
    FaceAttributeType.Blur,
    FaceAttributeType.Glasses
};
Python

Python
# Specify facial features to be retrieved
features = [FaceAttributeType.occlusion,
            FaceAttributeType.blur,
            FaceAttributeType.glasses]
In the DetectFaces function, under the code you just added, find the comment Get faces and add the following code:

C#

C
// Get faces
using (var imageData = File.OpenRead(imageFile))
{    
    var detected_faces = await faceClient.Face.DetectWithStreamAsync(imageData, returnFaceAttributes: features, returnFaceId: false);

    if (detected_faces.Count > 0)
    {
        Console.WriteLine($"{detected_faces.Count} faces detected.");

        // Prepare image for drawing
        Image image = Image.FromFile(imageFile);
        Graphics graphics = Graphics.FromImage(image);
        Pen pen = new Pen(Color.LightGreen, 3);
        Font font = new Font("Arial", 4);
        SolidBrush brush = new SolidBrush(Color.Black);
        int faceCount=0;

        // Draw and annotate each face
        foreach (var face in detected_faces)
        {
            faceCount++;
            Console.WriteLine($"\nFace number {faceCount}");

            // Get face properties
            Console.WriteLine($" - Mouth Occluded: {face.FaceAttributes.Occlusion.MouthOccluded}");
            Console.WriteLine($" - Eye Occluded: {face.FaceAttributes.Occlusion.EyeOccluded}");
            Console.WriteLine($" - Blur: {face.FaceAttributes.Blur.BlurLevel}");
            Console.WriteLine($" - Glasses: {face.FaceAttributes.Glasses}");

            // Draw and annotate face
            var r = face.FaceRectangle;
            Rectangle rect = new Rectangle(r.Left, r.Top, r.Width, r.Height);
            graphics.DrawRectangle(pen, rect);
            string annotation = $"Face ID: {face.FaceId}";
            graphics.DrawString(annotation,font,brush,r.Left, r.Top);
        }

        // Save annotated image
        String output_file = "detected_faces.jpg";
        image.Save(output_file);
        Console.WriteLine(" Results saved in " + output_file);   
    }
}
Python

Python
# Get faces
with open(image_file, mode="rb") as image_data:
    detected_faces = face_client.face.detect_with_stream(image=image_data,
                                                            return_face_attributes=features,                     return_face_id=False)

    if len(detected_faces) > 0:
        print(len(detected_faces), 'faces detected.')

        # Prepare image for drawing
        fig = plt.figure(figsize=(8, 6))
        plt.axis('off')
        image = Image.open(image_file)
        draw = ImageDraw.Draw(image)
        color = 'lightgreen'
        face_count = 0

        # Draw and annotate each face
        for face in detected_faces:

            # Get face properties
            face_count += 1
            print('\nFace number {}'.format(face_count))

            detected_attributes = face.face_attributes.as_dict()
            if 'blur' in detected_attributes:
                print(' - Blur:')
                for blur_name in detected_attributes['blur']:
                    print('   - {}: {}'.format(blur_name, detected_attributes['blur'][blur_name]))

            if 'occlusion' in detected_attributes:
                print(' - Occlusion:')
                for occlusion_name in detected_attributes['occlusion']:
                    print('   - {}: {}'.format(occlusion_name, detected_attributes['occlusion'][occlusion_name]))

            if 'glasses' in detected_attributes:
                print(' - Glasses:{}'.format(detected_attributes['glasses']))

            # Draw and annotate face
            r = face.face_rectangle
            bounding_box = ((r.left, r.top), (r.left + r.width, r.top + r.height))
            draw = ImageDraw.Draw(image)
            draw.rectangle(bounding_box, outline=color, width=5)
            annotation = 'Face ID: {}'.format(face.face_id)
            plt.annotate(annotation,(r.left, r.top), backgroundcolor=color)

        # Save annotated image
        plt.imshow(image)
        outputfile = 'detected_faces.jpg'
        fig.savefig(outputfile)

        print('\nResults saved in', outputfile)
Examine the code you added to the DetectFaces function. It analyzes an image file and detects any faces it contains, including attributes for age, emotions, and the presence of spectacles. The details of each face are displayed, including a unique face identifier that is assigned to each face; and the location of the faces is indicated on the image using a bounding box.

Save your changes and return to the integrated terminal for the face-api folder, and enter the following command to run the program:

C#

dotnet run
The C# output may display warnings about asynchronous functions now using the await operator. You can ignore these.

Python

python analyze-faces.py
When prompted, enter 1 and observe the output, which should include the ID and attributes of each face detected.

View the detected_faces.jpg file that is generated in the same folder as your code file to see the annotated faces.