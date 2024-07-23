# Gapi
## _The Integration Engine for Generative AI_

[Project Page](https://GenAINerds.com/#/Gapi)

[1 Min. Video Intro](https://www.youtube.com/watch?si=8Bt47WdUtiTaSZQx&v=6u7_-O1PCt8&feature=youtu.be)

[Docs](https://genainerds.com/#/Docs)

![Gif Teaser](https://genainerds.com/assets/img/GapiGIF.gif)

Goal: Be the Fastest Way to Prototype and Integrate GenAI on the Edge.
- Gapi is an integration engine designed for non AI experts. Leverage the leading, open GenAI models you hear about everyday.
- A graphical workflow editor and lots of out-of-the-box integrations with real world systems.
- Demo and pilot your creations fast to prove value and move your project forward.

## Supported Hosts
- Arm 64 w/ Docker support
- Will run on others in emulated mode

## Local Installation (Coming Soon)

Please create a free account on GenAIGapi.com for now to play around. We are getting the Gapi Server Docker image ready!

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

**Ollama**

Oh Yeah! Now in the GenAI LLM (Local) Component you can choose a model and Ollama will try and load it w/ unified interaction made possible by it and our plumbing!
Note: in ~/gapiData/ there is a Gapi-Ollama folder. In there is a config.txt file that has the websocket url for this container to connect to plus the Micro Service key. So once you start it up you should see it Online in the Micro Services tab of the Gapi UI.
Also, any models that are downloaded by Ollama in the container will be in this folder too
```sh
docker pull genainerds/gapi:ollama
docker create -it --runtime nvidia --name gapi-ollama --network=host -v ~/gapiData/Gapi-Ollama:/home/ollama/vdata -e OLLAMA_MODELS=/home/ollama/vdata/models -e OLLAMA_LOGS=/home/ollama/vdata/ollama.log genainerds/gapi:ollama /bin/bash -c "./home/ollama/start-all-gapi.sh"
docker start gapi-ollama
```

## Want to Wrap Your Code/model in a Micro Service and Use it in a Workflow?

Go to the Micro Services section of this: [Docs](https://genainerds.com/#/Docs)
