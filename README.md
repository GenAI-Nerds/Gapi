# Gapi
## _The Integration Engine for Generative AI_

![Gif Teaser](https://genainerds.com/assets/img/GapiGIF.gif)

Gapi is an embeddable API gateway that creates streaming integrations between AI micro services and the systems that users leverage everyday. "On Device" generative AI doesn't mean it has to live on an island!

Goal: Be the Fastest Way to Prototype and Integrate GenAI on the Edge.
- Gapi is an integration engine designed for non AI experts. Leverage the leading, open GenAI models you hear about everyday.
- Workflow engine with low code UI with dozens of open integrations and customizable clients for mobile web and desktop.
- Micro service framework for wrapping Jetson containers (Ollama, Whisper, Piper TTS, etc. are done, with more coming). Or wrap your own models/code and integrate it into Workflows.
- Real-time, hybrid, binary+json messaging smoothens intra-service calls and reduced latency.
- A fast path to proving generative AI value to stakeholders in their actual environment.

## Links
- [Project Page](https://GenAINerds.com/#/Gapi)
- [1 Min. Video Intro](https://www.youtube.com/watch?si=8Bt47WdUtiTaSZQx&v=6u7_-O1PCt8&feature=youtu.be)
- [Docs](https://genainerds.com/#/Docs)
- [Clients](https://github.com/orgs/GenAI-Nerds/repositories)

## Gapi Server:
Embeddable API gateway software that runs in the background with a low code workflow UI for testing. The server is a message hub and state machine for workflow 'nodes' that talk to Micro Services. Think of it as connective-tissue for applications.
![Gapi Server Diagram](https://genainerds.com/assets/img/GapiDiagram3.png)

Note: A Micro Service is a process that runs some wrapper python scripts that integrates custom code/models into Workflows using a streaming API.

Gapi Server can run on any Jetson Orin or really any computer as the Micro Services connect outbound over secure web sockets. It doesn't use any GPU resources. There is a also a little demo version to skip the Server install (but you'll still need to run your own Micro Services).

## What you need to run Gapi Server on NVIDIA Jetson
- Any Jetson Orin
- Docker
- Size: ~1.3GB

Gapi Server will run on other environments. Email us at support@GenAINerds.com if that's something you think is worthwhile.

## Installing Gapi Server (Step 1 of 2)
Explaining the Steps:

1) On the Docker host, create working dir for persistant data
2) Download configuration files
3) Unzip
4) Pull Docker image, create container and start the process (will return console to you)

Copy and Run the Steps:
```
sudo mkdir ~/gapiData && cd ~/gapiData
sudo curl -L https://raw.githubusercontent.com/genai-nerds/Gapi/main/gapiConfigs.zip -o gapiConfigs.zip
sudo unzip -q gapiConfigs.zip
sudo docker run -d --name gapi --network host -v ~/gapiData:/opt/gapi/vdata genainerds/gapi:arm64 /bin/bash -c "cd /opt/gapi/bin && ./startGapi.sh"
sudo echo "You may need to hit Enter now. Afterwards the Docker container 'gapi' should be running"
```

NOTE: You will need to run some Micro Services before doing anything meaningful, so please review the mini tour below but don't do any of it in the UI untill you complete the setup (instructions at the bottom)

## UI / Login
![UI](https://genainerds.com/assets/img/gapi-hero.png)
- Browse in: http://[host-device-ip]:8090
- User: root
- Pass: !gapi2024
- Change password in Settings! Docs shows how to add SSL cert.

## Running the Community Micro Services (Step 2 of 2)
```
#1 Login and go to the Micro Services tab
#2 Follow the instructions in the blue box on that page to download your custom configuration
#3 Then follow the instructions below that for installing the Micro Service you want
```
## Go Through the Workflow Tips
![Tips](https://genainerds.com/assets/img/WorkflowsHome.png)

## Creating Your Own Micro Service
The entire Micro Service zip file is just 4KB with 4 files:

message_handler.py: for you to respond
message.py: for the streaming binary/json protocol
gapi-ms: as entry point and handler)
requirements.txt: defines just asyncio + websockets
Synopsis below...
```
#1 Create logical Micro Service in UI and copy the key
#2 Download the zip file from the UI
#3 python gapi-ms.py ws://0.0.0.0:8090/gapi-ws [MICROSERVICE_KEY]
#4 Refresh the UI to confirm it's online
#5 Edit the message_handler.py to handle binary+json input and change the output
#6 Add a Micro Service Node to a Workflow and tie it to your Micro Service. Hit Test.
```

Troubleshooting:

Keep in mind all data read or written is in ~/gapiData
Look at ~/gapiData/gapi.log to see what happened (if say the docker run command doesn't work)
gapiServerConfig.json has all the initial setup
Post what the logs show as well as ~/gapiData/gapi.log

## Run on Boot
For now you can just run "docker start gapi" in your OS startup script

## Updating Gapi Server
Same steps as usual, kill running container, rm the container for space, pull new one and launch anew
```
sudo docker kill gapi
sudo docker rm gapi
sudo docker pull genainerds/gapi:arm64
sudo docker run -d --name gapi --network host -v ~/gapiData:/opt/gapi/vdata genainerds/gapi:arm64 /bin/bash -c "cd /opt/gapi/bin && ./startGapi.sh"
```

