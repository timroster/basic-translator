# Basic STT->Language Translator->TTS example

Ever wondered what it might take to have what you're speaking be translated on the fly to another language? This project does exactly that in a very simple way. The project uses the IBM Watson Speech to Text, Language Translator, and Text to Speech service in a simple flow. Spoken text is processed by Speech to Text to create a transcript, and then the transcript is processed by Language Translator, and finally sent over to Text to Speech.

The system is designed to work on a sentence-by-sentence basis. In order to do this, the speaker needs to have a brief pause at the end of each spoken sentence, allowing the Speech to Text service to finalize the transcription. When running on a terminal, the transcribed sentence is displayed to confirm that it's been processed and sent over for translation. After an initial delay to transcribe and translate the first sentence, following sentences will be delivered to listeners in a sequential fashion as long as the translated sentences are approximately the same length as the original sentence.

A more sophisticated orchestration system would extend this approach by processing the metadata output from Text to Speech to ensure that later translated sentences were queued by the appropriate time needed to playback the current sentence. Generally though, the pauses between spoken sentences are sufficient to prevent overlap or interruptions.

## Setup

1. On IBM Cloud, deploy Node-RED starter, bind instances of Language Translator and Speech To Text (preferrably standard) as Lite only supports 10k chars. Restage after binding.

1. Create instance of Text to Speech and make note of the api key.

1. Copy the `.env.example` file to `.env` and update with the Text to Speech api key and update the WSTARGET value to match the deployed Node-RED starter, appending /ws/text to the URL. Example: `https://translator-low-code-2019.mybluemix.net/ws/text`

1. Import the `translate-and-speak.json` flow into Node-RED.

1. Use npm to configure the local application that listens to the mic (tested on Mac OSX):

    ```bash
       npm install
       npm start
    ```

Once the application starts, speak each sentence clearly and at an even pace. Pause briefly at the end of the sentence and you will see the most recent sentence transcription appear to the console. When that appears, the local application is sending over a websocket to Node-RED the text, which is then procesed on IBM Cloud to produce the audio output.

To listen to the audio output, open in a browser the URL of the Node-RED starter with /audio appended to the host name. Multiple users can connect to the websocket output.

The example flow is configured to translate from English to Spanish and speak with the TTS female voice.
