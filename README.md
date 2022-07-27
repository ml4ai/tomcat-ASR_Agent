tomcat-ASRAgent
================

Agent that listens to the microphone and outputs messages corresponding to
real-time ASR transcriptions.

Example usage:

    ./asr_agent --mode websocket --mqtt_host "127.0.0.1" --mqtt_port 5556

To see all available options, run:

    ./asr_agent -h

## Prerequisites

* C++ compiler that supports C++17 or higher.
* nlohmann::json library: https://github.com/nlohmann/json
* Boost Library: https://www.boost.org/ 
* gRPC++ and Protobufers: https://grpc.io/docs/languages/cpp/quickstart/
* Mosquitto: https://mosquitto.org/

## Build process

    mkdir build
    cd build
    cmake ..
    make -j

### Google Cloud engine

To use the Google Cloud Speech Recognition engine, you will need to point the
environment variable GOOGLE_APPLICATION_CREDENTIALS to point to your Google
Cloud credentials file.

If using the Google Cloud Speech Recognition engine, you can add word/phrase "hints" 
to tell the speech regognizer to favor specific words and phrases. These should be
added to the conf/speech_context.txt file.
 
### Building locally on MACOS

If using MACOS, there is an additional step required to configure an environmental variable for gRPC:
'''
curl -Lo roots.pem https://pki.google.com/roots.pem
export GRPC_DEFAULT_SSL_ROOTS_FILE_PATH="$PWD/roots.pem"
'''

Docker instructions
-------------------
### Create the tomcat network:

The asr_agent uses an internal Docker network named tomcat to connect its components. It must be created before  running docker-compose:

	docker network create tomcat

### Google Cloud Speech Engine:

	docker-compose -f google.yml up --build

### Vosk Speech Engine:

	docker-compose -f vosk.yml up --build

By default, Docker will use connect to a mosquitto broker running on port 1883 of the host machine. If you are using a different Mosquitto broker, modify the mqtt_host and mqtt_port command line option in the asr_agent service entrypoint in google.yml or vosk.yml.

