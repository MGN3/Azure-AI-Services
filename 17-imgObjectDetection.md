# Detección de objetos en imágenes

Diferencia entre *image classification* y *object detection*
-La clasificación de imágenes escoge una de x etiquetas al conjunto de la imagen.
-La detección de objetos aplica una etiqueta de enter n etiquetas a cada uno de los objetos que puede detectar. Así como de una serie de coordenadas(region) que delimitan en un cuadrado el objeto.

## Entrenamiento para la detección de objetos
- Desde Custom Vision Studio con UI y asistente. Además se puede entrenar el modelo con **Smart labeler tool**.
- Azure Machine Learning Studio. Utilizar un recurso ML para entrenar(realizado en otro de los cursos, algo más largo y complejo en principio aunque probablemente el más efectivo/personalizable).
- Microsoft Visual Object Tagging Tool VOTT **(DEPRECATED)**
- Otras herramientas de etiquetado. En este caso, el output de esas herramientas debe cumplir con el sistema de medición aceptado por Custom Vision API.

```
	JSON:
	"tags": [
		{
		"tag": "manzana",

		"left": 0.1,

		"top": 0.5,

		"width": 0.5,

		"height": 0.25
		}
	]
```

# Laboratorio(ai-102 archivo 18)


- Crear recurso Custom Vision en Azure(opción ambas)
- Utilizar Custom Vision Portal, crear un proyecto y conectarlo con el recurso antes creado.
- Seleccionar en el proyecto el resto de configuraciones.
- Subir imágenes al proyecto desde el portal y etiquetarlas con asistencia smart tagging.
- Usar un programa C# para conectarnos con la API de Custom Vision Portal(endpoint, key, project ID).
- El programa creado debe realizar ciertas tareas para enviar las imágenes y los archivos JSON con las etiquetas y coordenadas de los objetos.(Así, se automatiza esta tarea o se puede hacer como parte de otro programa)
- Usar botón **train** para entrenar el modelo
- Probar el modelo con el botón quick test
- Publicar el modelo para que pueda ser usado por aplicaciones cliente. En el apartado Performance, ahí damos a publicar, seleccionando el recurso en Azure **CustomVision-Prediction**
- Una vez publicado, en un programa cliente o donde sea conveniente, insertamos **Endpoint** y **Key** para poder conectarnos al recurso CustomVision-Prediction.
- El programa podría enviar imágenes al servicio de predicción y devolverá el documento Json con las etiquetas y la regiones(coordenadas).
- El programa cliente podría luego generar con esas coordenadas y recuadro que delimite visualmente los objetos detectados, o cualquier otra acción con los datos del JSON, como simplemente guardar en una base de datos las etiquetas detectadas en cada imagen.

# Conclusiones sobre Azure Vision Studio y Custom Vision
Parecen existir varias formas de utilizar servicios de vision computacional de microsoft. El primero parece ser un servicio más accesible y sencillo para personas sin experiencia en ML. El segundo es más completo y enfocado a personas con algunos conocimientos de ML, además, requiere por tanto creaer recursos de Azure ML.(de todas formas, tampoco es necesario ser un experto en ML creo).

**Azure Cognitive Services - Custom Vision:**
Custom Vision es un servicio en la nube de Microsoft Azure que permite a los usuarios crear y entrenar modelos de visión computacional personalizados sin necesidad de experiencia previa en aprendizaje automático. Permite a las empresas y desarrolladores crear modelos de detección de objetos, clasificación de imágenes y detección de diferencias. El servicio se centra en tareas específicas de visión computacional y es ampliamente utilizado para aplicaciones de reconocimiento de imágenes y procesamiento de contenido visual. Custom Vision simplifica el proceso de entrenamiento y despliegue de modelos de visión computacional, lo que lo hace accesible a una audiencia más amplia.

**Azure Machine Learning - Visual Interface (Azure Vision Studio):**
Azure Vision Studio, también conocido como Azure ML Visual Interface, es una herramienta de Microsoft Azure que forma parte de Azure Machine Learning. Es una interfaz gráfica de usuario (GUI) que ayuda a los usuarios a crear y entrenar modelos de aprendizaje automático, incluyendo tareas de visión computacional, sin necesidad de escribir código. A través de la interfaz visual, los usuarios pueden arrastrar y soltar componentes, configurar parámetros y ejecutar flujos de trabajo de aprendizaje automático. Azure Vision Studio se utiliza para tareas más amplias de aprendizaje automático, no solo limitadas a la visión computacional.

# Preguntas
	1. What does an object detection model predict? 

The location and class of specific classes of object in an image.
That's correct. Object detection is used to identify bounding boxes containing specific classes of object in an image.


The class of the main subject of an image.

The file type of an image.

	2. What must you do before taking advantage of the smart labeler tool when creating an object detection model? 

Create a JSON file containing bounding box coordinates.

Tag some images with objects of each class and train an initial object detection model.
That's correct. To take advantage of the smart labeler, tag some images and train an initial model. Subsequently, the portal will suggest tags for new images.


Train an image classification (multilabel) model. 