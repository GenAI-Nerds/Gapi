# Gapi
## _The Integration Engine for Generative AI_

[Project Page](https://GenAINerds.com/#/Gapi)

[1 Min. Video Intro](https://www.youtube.com/watch?si=8Bt47WdUtiTaSZQx&v=6u7_-O1PCt8&feature=youtu.be)

![Gif Teaser](https://genainerds.com/assets/img/GapiGIF.gif)

Goal: Be the Fastest Way to Prototype and Integrate GenAI on the Edge.
- Gapi is an integration engine designed for non AI experts. Leverage the leading, open GenAI models you hear about everyday.
- A graphical workflow editor and lots of out-of-the-box integrations with real world systems.
- Demo and pilot your creations fast to prove value and move your project forward.

## Supported Hosts
- Arm 64 w/ Docker support
- Will run on others in emulated mode

## Installation

Steps to install Gapi (swap ~/gapiData in all the steps as you please)
0) Dependencies (Follow Docker install before moving forward)
```sh
    apt get -y install curl unzip
```
1) Create a place where your persistent data wil live
```sh
    mkdir ~/gapiData
```    
2) Download the starting json files
```sh
    curl -L https://raw.githubusercontent.com/genai-nerds/Gapi/main/gapiStartingData.zip -o ~/gapiData/gapiStartingData.zip
```    
3) Unzip them
```sh
    unzip ~/gapiData/gapiStartingData.zip
``` 
4) Pull it
```sh
    docker pull genainerds/gapi:arm64
```
5) Create a container, map network and bind the folder we created above
```sh
    docker create --name gapi --network host -v ~/gapiData:/opt/gapi/vdata genainerds/gapi:arm64 /bin/bash -c "cd /opt/gapi/bin && ./startGapi.sh"
```
6) Start the docker container
```sh
    docker start gapi
```

## Problems?
~/gapiData is your friend. It has all the persistant data plus gapi.log and gapi-error.log from the application server itself.
It also has folders for special Docker Micro Service that have the respective configuration and commensurate logging

Post what the logs show as well as ~/gapiData/conf/gapi.log
```sh
docker logs gapi
```

## Run on Boot
For now you can just run "docker start gapi" in your OS startup script

## Key / Default Micro Services via Docker (more coming)

**Piper TTS**

If you want to use the Text to Speech Component run the following commands. Add "docker start gapi-pipertts" to your startup script too!
Note: in ~/gapiData/ there is a Gapi-PiperTTS folder. In there is a config.txt file that has the websocket url for this container to connect to plus the Micro Service key. So once you start it up you should see it Online in the Micro Services tab of the Gapi UI.
```sh
docker pull genainerds/gapi:pipertts
docker create --runtime nvidia --name gapi-pipertts --network host -v ~/gapiData/Gapi-PiperTTS:/home/TTS/vdata genainerds/gapi:pipertts /bin/bash -c "cd /home/TTS && python3 gapi-ms.py [] []"
docker start gapi-pipertts
```
