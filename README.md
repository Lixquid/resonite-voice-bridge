# Resonite Voice Bridge

This application enables the use of Speech-To-Text in Resonite, by bridging Google Chrome's STT API with a Websocket server.

## Download

The latest version can be found on the [releases page](https://github.com/theneolanders/resonite-voice-bridge/releases).

## Usage

Launch the server executable, then open http://localhost:5000/ in Google Chrome. Grant the microphone permission and test the interface by speaking. You should see page saying the microhone is listening, the websocket is connected and your spoken text appearing.

In Resonite, use the Websocket Connect node to create a websocket connection to ws://localhost:6789/. Any speech the page detects will be sent to this websocket.

Use the Websocket Message Received node to receive real-time updates from the speech recognition.

## Commands and Events

The application supports commands and events to toggle and monitor the state of the microphone.

It is advised to disable the microphone access when not actively in use, as anything you say while the page is open and microphone is active is being sent to Google's servers.

You can send the following commands to the websocket connection to control the microphone state:

* _toggle_ - Toggles the microphone on and off
* _enable_ - Enables the microphone
* _disable_ - Disables the microphone

The server will send the following event messages when the microphone status changes:

* ^enabled^ - The microphone has been enabled
* ^disabled^ - The microhone has been disabled

Note the _ and ^ characters in the sent and received messages to simplify Protoflux parsing.

## How it works

Internally the script is hosting both a webserver for the interface, and a websocket server for Resonite to connect to.

The page you load uses Javascript to utilize Google's SpeechRecognition API via Chrome, and then sends that information to the websocket server.

The websocket server is configured to echo any message it receives back to all other connected clients.

## Troubleshooting

The text from the speech recognition API is streamed in real-time, rather than waiting for a pause and then sending the entire captured string. Due to the way Google's speech recognition works this will result in excessive messages.

For example saying "This is a test" resulted in 6 messages sent to the websocket:

![image](https://github.com/theneolanders/resonite-voice-bridge/assets/3112763/b9a624f5-7987-40a2-a8ac-39531735ced6)

If you're not getting speech transcription in the webpage, make sure Chrome is listening to the correct input device:

![image](https://github.com/theneolanders/resonite-voice-bridge/assets/3112763/25ea18ba-35d9-470a-b68e-68c06fc3983a)

Additionally try testing the Chrome speech API on a different site to verify it's working: https://mdn.github.io/dom-examples/web-speech-api/speech-color-changer/

## Building the executable

Install the pyinstaller package and then run it against server.py

`pip install pyinstaller && pyinstaller server.py`

Then copy the `static` and `templates` folders into the `_internal` folder in the `dist` output

# Disclaimer

This project is in no way affiliated with by Resonite or any member of its staff.
