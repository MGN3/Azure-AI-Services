 # Azure OpenAI Services
## Acceso limitado a empresas de Azure OpenAI
 Links con información extra:

**Paso a paso** para creación recurso Azure OpenAI: https://microsoftlearning.github.io/mslearn-generative-ai/Instructions/Labs/2-explore-azure-openai.html

Ejemplos de uso ChatGPT: https://platform.openai.com/examples

Diferencia entre Deep Learning y Automatic Learning(Machine Learning): https://learn.microsoft.com/es-es/azure/machine-learning/concept-deep-learning-vs-machine-learning?view=azureml-api-2

Esquema Servicios AI Microsoft: https://learn.microsoft.com/es-es/training/wwl-data-ai/explore-azure-openai/media/microsoft-ai-portfolio-graphic.png

## Elementos principales:
Azure OpenAI está disponible para los usuarios de Azure y consta de cuatro componentes:

	1- Modelos de IA generativos previamente entrenados
	2- Capacidades de personalización; la capacidad de ajustar los modelos de inteligencia artificial con sus propios datos
	3- Herramientas integradas para detectar y mitigar casos de uso dañinos para que los usuarios puedan implementar la inteligencia artificial de forma responsable
	4- Seguridad de nivel empresarial con control de acceso basado en rol (RBAC) y redes privadas

**(Punto 2: suele ser una demanda habitual de clientes)**

## Grupos de AI

### Servicios de AI para desarrolladores:
	1- Azure Machine Learning.
	2- Cognitive Services:
	Vision, Voice, Language, Decition, **OpenAI**
	3- Applied AI Services:
	Bot service, Cognitive Search, Form Recognizer, Video Indexer, Metrics Advisor, Immersive Reader.

Algunos servicios/funcionalidades de Cognitive Services y OpenAI están "superpuestos". Por ejemplo traducción, análisis de sentimiento y extracción de palabras clave, entre otros.

Es posible que a día de hoy los modelos de OpenAI hayan superado a otros servicios de Cognitive Services.

### Servicios de AI para no-code y low-code:
Dos subgrupos:

Aplicaciones:
	1- Microsoft 365
	2- Microsoft Dynamics 365
	3- Microsoft Edge
	4- Microsoft Bing
	5- Windows
	6- Xbox

Power platform **Quiza Microsoft Fabrick en un futuro(repasar MS build 2023)**:
	1- Power BI
	2- Power Apps
	3- Power Automate
	4- Power Virtual Agents


[[
**Precio estimado OpenAI:** 2000$ al mes de servicio: 5000 personas haciendo un uso intensivo a diario durante un mes.
Estimación:
10000 tokens por día por persona durante 30 días: 300.000 tokens al mes.

1.000.000.000 son 2000$ al mes de OpenAI.

1.000.000.000 / 300.000 = 3.333,33

Si quitamos fines de semana redondeo y que la estimación diaria de uso es alta: unos 5.000 usuarios al mes por 2.000$.

Estimación sin tener en cuenta otros costes añadidos por el uso de Azure u otros.
]]

### A la hora de decidir que servicio usar:
Existen muchos otros factores a tener en cuenta:

	- El tiempo necesario para preparar los datos y "limpiarlos".
	- El tiempo necesario para entrenar modelos.
	- Los tiempos de creación, despliegue y conexión de recursos(DevOps).
	- Los tiempos de desarrollo del producto o solución.

**Actualmente, Azure OpenAI solo está disponible para empresas/asociados bajo solicitud.**

## Servicios de OpenAI específicos:
Resumir texto, clasificar texto, lenguaje natural para SQL, generar nombres...

Los modelos como GPT3, GPT3.5 y GPT4 están optimizados para ciertas tareas específicas.

Todos los modelos de IA de Azure OpenAI se pueden entrenar y personalizar con ajuste preciso(prompt engineering aparte?).

Ajuste: https://learn.microsoft.com/es-es/azure/ai-services/openai/how-to/fine-tuning?pivots=programming-language-studio

### Playground Finalizaciones:
En este apartado se puede experimentar con los modelos, configurar parámetros y ver respuestas mediante una UI.

### Chat Playground:
Similar al anterior pero más enfocado al prompt engineering. Establecer valores requisitos previos para ser tenidos en cuenta a la hora de responder.
Parámetros:

	1- Implementaciones
	2- Tokens máximos
	3- Temperatura
	4- Principal
	...




