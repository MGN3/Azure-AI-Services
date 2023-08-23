## Azure OpenAI Service

https://learn.microsoft.com/es-es/azure/cognitive-services/openai/chatgpt-quickstart?tabs=command-line&pivots=programming-language-studio

## ANOMALY DETECTOR: API de detección de anomalías.

	https://azure.microsoft.com/es-es/products/cognitive-services/anomaly-detector/

	Anomaly Detector vs Metrics Advisor:
	 AD se compone de API REST sencillas con una experiencia que da prioridad a la programación. Es el motor principal de Metrics Advisor que detecta anomalías en los datos de series temporales. Es ideal para el análisis de datos ad hoc y se puede ejecutar en contenedores. 
	 
	 Metrics Advisor tiene características adicionales de supervisión de series temporales, con API de canalizaciones y una interfaz de usuario integrada para administrar el servicio. Está diseñado para datos de streaming en vivo y el análisis con inteligencia artificial, y se puede implementar en Azure.

## Seeing AI: 

	https://www.microsoft.com/en-us/ai/seeing-ai?rtc=1

	Aprovecha la eficacia de la inteligencia artificial para abrir el mundo visual y describir personas, texto y objetos cercanos.

	1- Clasificación de imágnees
	2- Detección de objetos
	3- Segmentación semántica
		La segmentación semántica es una técnica avanzada de aprendizaje automático en la que los píxeles individuales de la imagen se clasifican según el objeto al que pertenecen. Por ejemplo, una solución de control del tráfico podría superponer imágenes de tráfico con capas de "máscara" para resaltar diferentes vehículos mediante colores concretos.
	4- Análisis de imágenes
	5- Detección analisis y reconocimiento de caras.
	6- Reconocimiento óptico de caracteres.OCR

	Servicios de visión artificial Azure
	1- Computer vision: analiza img/vd extrae descripciones, etiquetas, texto..
	2- Custom Vision: clasifica img y detecta objetos personalizados mediante imágenes propias.
	3- Face: Detección de caras y personas.
	4- Form Recognizer: Información facturas y formularios escaneados.

## Procesamiento natural del lenguaje Azure
	1- Lenguaje: Comprender y analizar texto, entrenar modelos para comprender comandos de texto/voz y compilar aplicaciones inteligentes.
	2- Traductor: Traducción a 60 idiomas.
	3- Voz: Reconocer y sintetizar mensajes de voz y traducir idiomas hablados.
	4- Azure Bot: AI conversacional, un agente de software que participa en conversaciones.
**Los desarrolladores pueden usar Bot Framework** para crear un bot y administrarlo con Azure Bot Service, para integrar servicios backend como Language, y conectarse a canales de chat web, email, Teams...

## Minería de conocimiento:	
	Producto: Azure Cognitive Search.

	Soluciones que implican la extracción de información de grandes volúmenes de datos a menudo no estructurados para crear un almacén de conocimiento indexado.

	Pueden utilizarse otros servicios azure/AI de Cognitive Services para minar datos de imágenes, texto, lenguaje natural procesado...
	
## Desafíos y riesgos al aplicar soluciones AI - SOCIOTECHNICAL CHALLENGES
	1- **Sesgo de datos:** sesgo de resultado
	Si los datos contienen sesgos como la discriminación en base a sexo, nacionalidad, etnia..., esto se reflejará en el output de la solución AI.
	2- Errores y daños: sistemas en tiempo real como un vehículo autonomo generan peligros si suceden errores.
	3- Exposición de datos: entrenar AI con datos confidenciales(pacientes médicos).
	4- Soluciones AI incompletas por diversidad de capacidades del usuario o por el contexto de una empresa.
	5- Confianza en los resultados de un servicio AI respecto a problemas complejos.

	6- Responsabilidad por errores en un servicio AI.
	Se reconoce facialmente de forma errónea a una persona y esto influye en un juicio. (Aplicable a cualquier error).

	////
	
	El caso del reconocimiento facial es muy representativo de los riesgos en el uso de AI, pues se pueden obtener datos emocionales, étnicos, religiosos, políticos... y los modelos pueden adquirir sesgos que existen el la sociedad si el aprendizaje recibido no ha sido imparcial o incorrecto?
	Los servicios AI deberían ser lo más imparciales posible e influir en la creación de una sociedad menos afectada por la discriminación.

	Imparcialidad
	No discriminación
	Infrarepresentación
	Racismo


	Muchos elementos injustos de la sociedad podrían ser heredados por la AI si no son entrenados teniendo en cuenta muchos elementos complejos.

## Confiabilidad, seguridad y privacidad:
	En algunos sectores es clave por suponer un riesgo sustancial para:
	1- La vida humana
	2- La vida animal
	3- El medio y los ecosistemas 
	4- Infraestructuras clave

Es fundamental someter a los sistemas de SF e AI a procesos de **PRUEBA** antes del lanzamiento.

### Privacidad: 

	Los modelos se entrenan con datos que pueden incluir datos personales. Estos deben permanecer privados.

	1- Origen y lineage de los datos
	2- ¿Fuente pública o privada?
	3- Corrupción de datos
	4- Posibles influencias en los datos con los que se entrenan los modelos.

## Responsabilidad en modelos AI:

	Los equipos de desarrolladores de empresas que crean soluciones AI deben encargarse de que la solución cumpla con:
	- Estándares éticos.
	- Estándares Legales

	Cada parte del proceso de desarrollo debe tener en cuenta la normativa y principios aplicables.

**INVESTIGAR SOBRE LEGISLACIÓN APLICABLE**
	LINKS: 

	Responsabilidad general con MUCHAS guias y artículos. También se incluyen referencias a soluciones para afrontar estos problemas:
	https://www.microsoft.com/en-us/ai/responsible-ai-resources?rtc=1

	Responsabilidad sobre Seeing AI y facial recognition:
	https://www.microsoft.com/en-us/ai/responsible-ai-resources?rtc=1

## Conclusiones:

La AI es una tecnología más y al igual que en el mundo del desarrollo y las IT y en general como principio económico es un ejemplo de reusabilidad.

Ahora mismo no es necesario desarrollar desde cero soluciones de software, para eso se crearon ecosistemas de paquetes y dependencias.

Tampoco es necesario crear una web desde cero, existen frameworks y plantillas que agilizan el proceso. Al igual que estándares para el back-end y las comunicaciones en red.

Las inteligencias artificiales no son enemigos de los desarrolladores, pues los desarrolladores crean soluciones de software, independientemente de cuanto código tienen que escribir o cuantas otras soluciones intermedias crear.

A veces, basta con usar código para unir los puntos, para conectar APIs y aplicar soluciones existentes a problemas específicos y hacer fine-tuning para ese caso concreto.

Existen numerosas soluciones existentes que emulan las capacidades humanas o las superan en muchos aspectos:
	1- Análisis de texto, imágenes, video y documentos.
	2- Extracción de datos y conclusiones sobre los datos capturados/aplicados.
	3- Outputs variables dependiendo de que se busca. 
	Bot conversacional, detección de anomalías, clasificación de objetos, ordenación de datos, predicciones, conclusiones y toma de decisiones.

Todas estas funcionalidades conllevan riesgos que deben ser tenidos en cuenta para generar productos confiables que cumplen con la normativa y principios éticos.