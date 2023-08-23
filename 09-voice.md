# Voice AI Service
Objetivos:
	- Aprovisionamiento de un recurso de Azure para el servicio de Voz
	- Uso de la API Speech-to-Text para implementar el reconocimiento de voz
	- Uso de la API Text-to-Speech para implementar la síntesis de voz
	- Configuración del formato de audio y las voces
	- Uso de Lenguaje de marcado de síntesis de voz (SSML)


## APIs de Voice
	- Speech to Text
	- Text to Speech
	- Speech Translation
	- Speaker Recognition
	- Intent Recognition(integrado con Language Understanding)

# Speech to text
Dos APIs:

	- Speech to Text.
	- Speech to Text Short Audio(optimizada para audio 60s).

Estrategias principales:

	- Usar una u otra
	- Usar la la normal o la Short en base a la duración del audio.
	- Usar la normal para varios archivos por batchs(lotes).

### En la práctica, la mayoría de las aplicaciones interactivas habilitadas para voz usan el servicio de Voz a través de un SDK específico del lenguaje (programación).

![SpeechApiPattern](https://learn.microsoft.com/es-es/training/wwl-data-ai/transcribe-speech-input-text/media/speech-to-text.png)


## Uso SDK Speech-to-Text

Para utilizar el servicio es necesario implementar las clases/objetos:

- **SpeechConfig**. encapsula credenciales, endpoint y key.
- **AudioConfig**: define el origen de entrada de audio. Uso del predeterminado por el sistema(pero se puede especificar un archivo).
- **SpeechRecognizer**. Se construye con los dos anteriores objetos. Se trata de un cliente proxy para Speech-to-Text.
- **RecognizeOnceAsync()**. Método de la clase SpeechRecognizer. Usa el servicio Voice para transcribir audio a texto de forma asíncrona.
- **SpeechRecognitionResult**. Objeto devuelto por el anterior método.

Propiedades del objeto devuelto por el anterior método:

	1- Duration
	2- OffsetInTicks
	3- Propiedades
	4- Motivo
	5- ResultId
	6- Texto

 # Text-to-Speech
Dos APIs:

	- Text-to-Speech
	- Text-to-Speech Long Audio(para operacione por batch/lotes para mucho texto)

En este imagen se detallan los objetos/clases a implementar, igual o similar a la API anterior.

![SpeechApiPattern2](https://learn.microsoft.com/es-es/training/wwl-data-ai/transcribe-speech-input-text/media/text-to-speech.png)

El objetro SpeechConfig permite modificar el formato de salida de audio/voz generada.

Opciones:

- Tipo de archivo de audio
- Frecuencia de muestreo
- Profundidad de bits.

Ejemplo de formato:

speechConfig.SetSpeechSynthesisOutputFormat(SpeechSynthesisOutputFormat.**Riff24Khz16BitMonoPcm**);

### Otros formatos en el link:
https://learn.microsoft.com/es-es/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-dotnet

### Voces
- Voces estandar: sintéticas creadas a partir de ejemplos de audio.

- Voces neuronales: voces con un sonido más natural creadas mediante redes neuronales profundas.

Ejemplo:
speechConfig.SpeechSynthesisVoiceName = "**en-GB-George**";

Link con todas las voces posibles:
https://learn.microsoft.com/es-es/azure/ai-services/speech-service/language-support?tabs=stt#text-to-speech


## Uso de SSML(xml para voz)

Para que texto en este formato sea sintetizado, se puede usar:
**speechSynthesizer.SpeakSsmlAsync(ssml_string);**

```
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" 
    xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US"> 
    <voice name="en-US-AriaNeural"> 
        <mstts:express-as style="cheerful"> 
          I say tomato 
        </mstts:express-as> 
    </voice> 
    <voice name="en-US-GuyNeural"> 
        I say <phoneme alphabet="sapi" ph="t ao m ae t ow"> tomato </phoneme>. 
        <break strength="weak"/>Lets call the whole thing off! 
    </voice> 
</speak>
```

# Conclusiones
Aunque Azure tiene un servicio propio de Voice, se puede hacer con cognitive services sin especificar nada más (útil).
Las dos credenciales son la **clave y región(westeurope)**.

El uso de la API parece ser sencillo una vez se tiene el esqueleto principal. De forma que se puede configurar facilmente el texto a reconocer, la salida sintetizada y cualquier otro string que queramos que sea leido.

Existen diferentes voces para diferentes idiomas y acentos, las mejores parecen ser las neuronales.

También se pueden modificar las voces para crear una "propia".
Link para modificar la voz de forma personalizada.
https://learn.microsoft.com/es-es/azure/ai-services/speech-service/custom-speech-overview

# Laboratorio


# --------------------------
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
When the resource has been deployed, go to it and view its Keys and Endpoint page. You will need one of the keys and the location in which the service is provisioned from this page in the next procedure.
Prepare to use the Speech service
In this exercise, you'll complete a partially implemented client application that uses the Speech SDK to recognize and synthesize speech.

Note: You can choose to use the SDK for either C# or Python. In the steps below, perform the actions appropriate for your preferred language.

In Visual Studio Code, in the Explorer pane, browse to the 07-speech folder and expand the C-Sharp or Python folder depending on your language preference.

Right-click the speaking-clock folder and open an integrated terminal. Then install the Speech SDK package by running the appropriate command for your language preference:

C#

dotnet add package Microsoft.CognitiveServices.Speech --version 1.28.0
Python

pip install azure-cognitiveservices-speech==1.28.0
View the contents of the speaking-clock folder, and note that it contains a file for configuration settings:

C#: appsettings.json
Python: .env
Open the configuration file and update the configuration values it contains to include an authentication key for your cognitive services resource, and the location where it is deployed. Save your changes.

Note that the speaking-clock folder contains a code file for the client application:

C#: Program.cs
Python: speaking-clock.py
Open the code file and at the top, under the existing namespace references, find the comment Import namespaces. Then, under this comment, add the following language-specific code to import the namespaces you will need to use the Speech SDK:

C#

C#
// Import namespaces
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
Python

Python
# Import namespaces
import azure.cognitiveservices.speech as speech_sdk
In the Main function, note that code to load the cognitive services key and region from the configuration file has already been provided. You must use these variables to create a SpeechConfig for your cognitive services resource. Add the following code under the comment Configure speech service:

C#

C#
// Configure speech service
speechConfig = SpeechConfig.FromSubscription(cogSvcKey, cogSvcRegion);
Console.WriteLine("Ready to use speech service in " + speechConfig.Region);

// Configure voice
speechConfig.SpeechSynthesisVoiceName = "en-US-AriaNeural";
Python

Python
# Configure speech service
speech_config = speech_sdk.SpeechConfig(cog_key, cog_region)
print('Ready to use speech service in:', speech_config.region)
Save your changes and return to the integrated terminal for the speaking-clock folder, and enter the following command to run the program:

C#

dotnet run
Python

python speaking-clock.py
If you are using C#, you can ignore any warnings about using the await operator in asynchronous methods - we'll fix that later. The code should display the region of the speech service resource the application will use.

Recognize speech
Now that you have a SpeechConfig for the speech service in your cognitive services resource, you can use the Speech-to-text API to recognize speech and transcribe it to text.

If you have a working microphone
In the Main function for your program, note that the code uses the TranscribeCommand function to accept spoken input.

In the TranscribeCommand function, under the comment Configure speech recognition, add the appropriate code below to create a SpeechRecognizer client that can be used to recognize and transcribe speech using the default system microphone:

C#

C#
// Configure speech recognition
using AudioConfig audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using SpeechRecognizer speechRecognizer = new SpeechRecognizer(speechConfig, audioConfig);
Console.WriteLine("Speak now...");
Python

Python
# Configure speech recognition
audio_config = speech_sdk.AudioConfig(use_default_microphone=True)
speech_recognizer = speech_sdk.SpeechRecognizer(speech_config, audio_config)
print('Speak now...')
Now skip ahead to the Add code to process the transcribed command section below.


# ALTERNATIVO A USO DE MICRO.
Alternatively, use audio input from a file
In the terminal window, enter the following command to install a library that you can use to play the audio file:

C#

dotnet add package System.Windows.Extensions --version 4.6.0 
Python

pip install playsound==1.2.2
In the code file for your program, under the existing namespace imports, add the following code to import the library you just installed:

C#

C#
using System.Media;
Python

Python
from playsound import playsound
In the Main function, note that the code uses the TranscribeCommand function to accept spoken input. Then in the TranscribeCommand function, under the comment Configure speech recognition, add the appropriate code below to create a SpeechRecognizer client that can be used to recognize and transcribe speech from an audio file:

C#

C#
// Configure speech recognition
string audioFile = "time.wav";
SoundPlayer wavPlayer = new SoundPlayer(audioFile);
wavPlayer.Play();
using AudioConfig audioConfig = AudioConfig.FromWavFileInput(audioFile);
using SpeechRecognizer speechRecognizer = new SpeechRecognizer(speechConfig, audioConfig);
Python

Python
# Configure speech recognition
audioFile = 'time.wav'
playsound(audioFile)
audio_config = speech_sdk.AudioConfig(filename=audioFile)
speech_recognizer = speech_sdk.SpeechRecognizer(speech_config, audio_config)
Add code to process the transcribed command
In the TranscribeCommand function, under the comment Process speech input, add the following code to listen for spoken input, being careful not to replace the code at the end of the function that returns the command:

C#

C#
// Process speech input
SpeechRecognitionResult speech = await speechRecognizer.RecognizeOnceAsync();
if (speech.Reason == ResultReason.RecognizedSpeech)
{
    command = speech.Text;
    Console.WriteLine(command);
}
else
{
    Console.WriteLine(speech.Reason);
    if (speech.Reason == ResultReason.Canceled)
    {
        var cancellation = CancellationDetails.FromResult(speech);
        Console.WriteLine(cancellation.Reason);
        Console.WriteLine(cancellation.ErrorDetails);
    }
}
Python

Python
# Process speech input
speech = speech_recognizer.recognize_once_async().get()
if speech.reason == speech_sdk.ResultReason.RecognizedSpeech:
    command = speech.text
    print(command)
else:
    print(speech.reason)
    if speech.reason == speech_sdk.ResultReason.Canceled:
        cancellation = speech.cancellation_details
        print(cancellation.reason)
        print(cancellation.error_details)
Save your changes and return to the integrated terminal for the speaking-clock folder, and enter the following command to run the program:

C#

dotnet run
Python

python speaking-clock.py
If using a microphone, speak clearly and say "what time is it?". The program should transcribe your spoken input and display the time (based on the local time of the computer where the code is running, which may not be the correct time where you are).

The SpeechRecognizer gives you around 5 seconds to speak. If it detects no spoken input, it produces a "No match" result.

If the SpeechRecognizer encounters an error, it produces a result of "Cancelled". The code in the application will then display the error message. The most likely cause is an incorrect key or region in the configuration file.

Synthesize speech
Your speaking clock application accepts spoken input, but it doesn't actually speak! Let's fix that by adding code to synthesize speech.

In the Main function for your program, note that the code uses the TellTime function to tell the user the current time.

In the TellTime function, under the comment Configure speech synthesis, add the following code to create a SpeechSynthesizer client that can be used to generate spoken output:

C#

C#
// Configure speech synthesis
speechConfig.SpeechSynthesisVoiceName = "en-GB-RyanNeural";
using SpeechSynthesizer speechSynthesizer = new SpeechSynthesizer(speechConfig);
Python

Python
# Configure speech synthesis
speech_config.speech_synthesis_voice_name = "en-GB-RyanNeural"
speech_synthesizer = speech_sdk.SpeechSynthesizer(speech_config)
Note: The default audio configuration uses the default system audio device for output, so you don't need to explicitly provide an AudioConfig. If you need to redirect audio output to a file, you can use an AudioConfig with a filepath to do so.

In the TellTime function, under the comment Synthesize spoken output, add the following code to generate spoken output, being careful not to replace the code at the end of the function that prints the response:

C#

C#
// Synthesize spoken output
SpeechSynthesisResult speak = await speechSynthesizer.SpeakTextAsync(responseText);
if (speak.Reason != ResultReason.SynthesizingAudioCompleted)
{
    Console.WriteLine(speak.Reason);
}
Python

Python
# Synthesize spoken output
speak = speech_synthesizer.speak_text_async(response_text).get()
if speak.reason != speech_sdk.ResultReason.SynthesizingAudioCompleted:
    print(speak.reason)
Save your changes and return to the integrated terminal for the speaking-clock folder, and enter the following command to run the program:

C#

dotnet run
Python

python speaking-clock.py
When prompted, speak clearly into the microphone and say "what time is it?". The program should speak, telling you the time.

Use a different voice
Your speaking clock application uses a default voice, which you can change. The Speech service supports a range of standard voices as well as more human-like neural voices. You can also create custom voices.

Note: For a list of neural and standard voices, see Language and voice support in the Speech service documentation.

In the TellTime function, under the comment Configure speech synthesis, modify the code as follows to specify an alternative voice before creating the SpeechSynthesizer client:

C#

C#
// Configure speech synthesis
speechConfig.SpeechSynthesisVoiceName = "en-GB-LibbyNeural"; // change this
using SpeechSynthesizer speechSynthesizer = new SpeechSynthesizer(speechConfig);
Python

Python
# Configure speech synthesis
speech_config.speech_synthesis_voice_name = 'en-GB-LibbyNeural' # change this
speech_synthesizer = speech_sdk.SpeechSynthesizer(speech_config)
Save your changes and return to the integrated terminal for the speaking-clock folder, and enter the following command to run the program:

C#

dotnet run
Python

python speaking-clock.py
When prompted, speak clearly into the microphone and say "what time is it?". The program should speak in the specified voice, telling you the time.

Use Speech Synthesis Markup Language
Speech Synthesis Markup Language (SSML) enables you to customize the way your speech is synthesized using an XML-based format.

In the TellTime function, replace all of the current code under the comment Synthesize spoken output with the following code (leave the code under the comment Print the response):

C#

C#
// Synthesize spoken output
string responseSsml = $@"
    <speak version='1.0' xmlns='http://www.w3.org/2001/10/synthesis' xml:lang='en-US'>
        <voice name='en-GB-LibbyNeural'>
            {responseText}
            <break strength='weak'/>
            Time to end this lab!
        </voice>
    </speak>";
SpeechSynthesisResult speak = await speechSynthesizer.SpeakSsmlAsync(responseSsml);
if (speak.Reason != ResultReason.SynthesizingAudioCompleted)
{
    Console.WriteLine(speak.Reason);
}
Python

Python
# Synthesize spoken output
responseSsml = " \
    <speak version='1.0' xmlns='http://www.w3.org/2001/10/synthesis' xml:lang='en-US'> \
        <voice name='en-GB-LibbyNeural'> \
            {} \
            <break strength='weak'/> \
            Time to end this lab! \
        </voice> \
    </speak>".format(response_text)
speak = speech_synthesizer.speak_ssml_async(responseSsml).get()
if speak.reason != speech_sdk.ResultReason.SynthesizingAudioCompleted:
    print(speak.reason)
Save your changes and return to the integrated terminal for the speaking-clock folder, and enter the following command to run the program:

C#

dotnet run
Python

python speaking-clock.py
When prompted, speak clearly into the microphone and say "what time is it?". The program should speak in the voice that is specified in the SSML (overriding the voice specified in the SpeechConfig), telling you the time, and then after a pause telling you it's time to end this lab - which it is!
