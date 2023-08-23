# Compilación de un modelo de Language Understanding.

## Objetivos
Aprovisionamiento de recursos de Azure para Language Understanding
Definición de intenciones, expresiones y entidades
Uso de patrones para diferenciar expresiones similares
Uso de componentes de entidad pregeneradas
Entrenamiento, prueba, publicación y revisión de un modelo de Language Understanding

## Caracteristicas de Language Undersanding
	- Analisis de sentimiento
	- Extracción de frases clave
	- Reconocimiento de entidades
	- Reconocimeinto de intenciones
	- Clasificación de texto
Para Clasificación de texto personalizada y Reconocimiento de entidades con nombre personalizado: https://learn.microsoft.com/es-es/training/paths/build-custom-text-analytics/

Web especial Language Studio:
https://language.cognitive.azure.com/

Link específico Azure AI Language:
https://azure.microsoft.com/en-us/products/ai-services/ai-language

Ejemplos de implementación de Language Services: https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/textanalytics/Azure.AI.TextAnalytics/samples/Sample2_AnalyzeSentiment.md


## Apunte
La creación de un recurso Azure AI services auna en un único recurso varios sercicios de AI. Se crea un único EndPoint y claves.

A partir de julio de 2023, los servicios de **Azure AI** abarcan todo lo que anteriormente se conocía como Azure Cognitive Services y Azure Applied AI Services

## Detección de intenciones
Un servicio de Language Understanding(LUIS) se encarga realizar diversas tareas sobre el texto normalmente input de un usuario.

LUIS y otros servicios van a ser reemplazados por una versión más completa denominada **"Azure AI Language"** o simplemente **"Language".**

## Entidades
Los modelos de comprensión de lenguaje pueden trabajar con **entidades**.

Ejemplos de entidades

	- Porcentajes
	- Dimensiones(distancia, volumen...)
	- Currency
	- Nombres de personas
	- Emails/url
	- Intervalos numéricos
Estas entidades vienen predefinidas y los modelos han sido pre-entrenados para **detectar** estos elementos expresados de diversas formas.
Así, no es necesario añadir ejemplos para el entrenamiento del modelo.

Para agregar entidades ya creadas y entrenadas:
**Agregar nueva precompilación**

Puede tener hasta cinco componentes precompilados por entidad. 

## Entrenamiento, prueba, publicación y revisión

	1- Entrenar un modelo para aprender intenciones y entidades a partir de expresiones de ejemplo.
	2- Prueba interactiva del modelo o uso de un conjunto de datos de prueba con etiquetas conocidas
	3- Implementación de un modelo entrenado en un punto de conexión público para que las aplicaciones cliente puedan usarla
	4- Revisar las predicciones e iterar en las expresiones para entrenar el modelo


# Laboratorio
No disponible, probablemente porque la unidad es sobre LUIS, deprecada.

# Test

1. La aplicación debe interpretar un comando como "activar la luz" o "encender la luz". ¿Qué representan estas frases en un modelo de lenguaje? 

Intenciones.
No es correcto. Una intención es un posible significado semántico que debe identificar el modelo.


Expresiones.
Correcto. Las expresiones son frases de ejemplo que indican una intención específica.


Entidades.
2. La aplicación debe interpretar un comando para reservar un vuelo a una ciudad especificada, como "Reservar un vuelo a París". ¿Cómo se debe modelar el elemento "city" (ciudad) del comando? 

Como intención.

Como expresión.

Como entidad.
Correcto. La ciudad es una entidad a la que se debe aplicar la intención (reservar un vuelo).

3. El modelo de lenguaje debe detectar un correo electrónico cuando está presente en una expresión. ¿Cuál es la manera más sencilla de extraer ese correo electrónico? 

Mediante entidades de expresión regular.

Uso de componentes de entidad pregeneradas.
Correcto. Cuando un modelo de lenguaje necesita detectar una entidad común, use componentes creados previamente para que el servicio language detecte automáticamente la entidad.


Uso de componentes de entidad aprendidos.
