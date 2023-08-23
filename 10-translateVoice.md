# Traducción de voz con el servicio de Voice.
Objetivos:
- Aprovisionar recursos de Azure para la traducción de voz.
- Generar traducción de texto a partir de voz.
- Sintetizar traducciones habladas.

Requisitos:
- Disponer de servicio Voice o Cognitive Services multiple.(migreaciones desde bing Speech y otros disponibles)
- Ubicación (westeurope)
- Key

## Patrón
El patrón es similar a los servicios de Voz sin traducción.

Hay que añadir un objeto **SpeechTranslationConfig**

Creación de un objeto **TranslationRecognizer** y uso del método RecognizeOnceAsync() para usar el servicio asíncronamente.

### Resultado: SpeechRecognitionResult
Este objeto incluye las siguientes propiedades:

- Duration
- OffsetInTicks
- Propiedades
- Motivo
- ResultId
- Texto
- Translations

![translateVoice](https://learn.microsoft.com/es-es/training/wwl-data-ai/translate-speech-speech-service/media/translate-speech.png)

## Síntesis de traducciones
Es posible crear traducción voz a voz.

	1- Especificar en el objeto TranslationConfig la voz deseada para la voz traducida.
	2- Crear un controlador de eventos para el evento Synthesizing del objeto TranslationRecognizer.
	3- En el controlador de eventos, utilice el método GetAudio() del parámetro Result para recuperar la secuencia de bytes del audio traducido.

### Síntesis manual
La síntesis manual es un enfoque alternativo a la síntesis basada en eventos que no requiere implementar un controlador de eventos. 

	1- Usa un objeto TranslationRecognizer para traducir la entrada hablada a transcripciones de texto en uno o varios idiomas de destino.
	2- Recorre en iteración el diccionario Translations en el resultado de la operación de traducción, con un objeto SpeechSynthesizer para sintetizar una secuencia de audio para cada idioma.

# Conclusiones

# Laboratorio.