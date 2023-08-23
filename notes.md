# ¿Como se que servicio específico usar para una funcionalidad concreta?
En las practicas se suele importar el SDK de varios servicios, en el de face se usan:

- ComputerVision para Face detection
dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 6.0.0

- Face
dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.6.0-preview.1

# Cuando usar SDK Azure(u otros SDK)

Es útil utilizarlo en la mayoría de casos de uso, pues facilita la comunicación mediante HTTP y JSON de forma más automática y con manejo de errores y controles de seguridad incluidos.

- Rapidez en el desarrollo
- Seguidad
- Manejo de errores
- Rendimiento
- Abstracciones de alto nivel

# Cuando podría ser mejor no utilizar un SDK:

## Flexibilidad total:
Utilizar solicitudes HTTP y JSON de forma manual permite tener un control absoluto sobre el proceso de comunicación con el servicio Azure. Puede ser útil cuando se requiere realizar operaciones personalizadas o específicas que no están cubiertas por el SDK.

## Entornos o lenguajes no compatibles:
Si estás trabajando en un entorno o lenguaje de programación que no cuenta con un SDK oficial de Azure, puedes optar por realizar solicitudes HTTP y procesar las respuestas JSON manualmente para interactuar con el servicio.

## Casos de baja complejidad:
En situaciones sencillas donde las interacciones con el servicio Azure son bastante directas y no requieren una gran cantidad de operaciones complejas, puede ser más práctico y liviano utilizar solicitudes HTTP y JSON en lugar de cargar un SDK completo.

## Reducción de dependencias: 
Si estás trabajando en un proyecto donde se intenta mantener el número de dependencias externas bajo control, evitar el uso de un SDK puede ser preferible.

## Escenarios muy específicos

# Decisiones sobre dependencias

	1- Importar todo el SDK y sus dependencias asociadas
	2- Importar solo dependencias específicas.

Para hacer lo segundo, buscar y agregar los NuGet individuales de funcionalidades específicas.

Especificarlo en .csproj o packages.config.

También existen dependencias transitivas aunque se incluyan depencencias específicas.

```
Install-Package Microsoft.Azure.Storage.Blob
```

Después en el archivo concreto del código usar: 
```
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;
```

El uso de solo algunas de las funcionalidades de un SDK o librería mediante *using* implica que el resto de las clases y dependencias no estarán cargadas en memoria. 
Solo las importadas serán cargadas, y el resto descartadas mediante la técnica de **"Dead Code Elimination / tree shaking"** realizado por el compilador **Roslyn**.

Las librerías estarán incluidas en su totalidad en los archivos del proyecto, pero solo las importadas / using serán las que se "carguen".

# UI usadas
- Azure Portal
- Language Studio(podría ser reemplazado al estar OpenAI?)
- Vision Studio
- ML Studio para hacer data labeling de imagenes con MML assisted labeling para agilizar el proceso.

## Servicios Azure limitados

The following services are Limited Access:

Custom Neural Voice: Pro features
Speaker Recognition: All features
Face API: Identify and Verify features, face ID property
Azure AI Vision: Celebrity Recognition feature
Azure AI Video Indexer: Celebrity Recognition and Face Identify features
Azure OpenAI: Azure OpenAI Service, modified abuse monitoring, and modified content filters

https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-limited-access

# Ideas:
- Examinar titulares/texto de noticias de diferentes medios sobre un tema específico y analizar el sentiment, hallando una media de positivo/negativo/neutral y demaás datos.
(podría hacerse lo mismo con diferentes fuentes de información, como video para telediarios o voz para radio...).

- Idea Computer Vision Parkings: para la detección de matrículas en parkings, ya existe pero podría ser un nicho de negocio pues aun hay mucho parkings que no leen matrículas. El servicio sería un OCR.