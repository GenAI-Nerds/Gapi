# Gapi
## _The Integration Engine for Generative AI_

![Gif Teaser](https://genainerds.com/assets/img/GapiGIF.gif)

Goal: Be the Fastest Way to Prototype and Integrate GenAI on the Edge.
- Gapi is an integration engine designed for non AI experts. Leverage the leading, open GenAI models you hear about everyday.
- A graphical workflow editor and lots of out-of-the-box integrations with real world systems.
- Demo and pilot your creations fast to prove value and move your project forward.

[Project Page](https://GenAINerds.com/#/Gapi)

[1 Min. Video Intro](https://www.youtube.com/watch?si=8Bt47WdUtiTaSZQx&v=6u7_-O1PCt8&feature=youtu.be)

[Docs](https://genainerds.com/#/Docs)

[Clients](https://github.com/orgs/GenAI-Nerds/repositories)

## What you need to run Gapi Server on NVIDIA Jetson
- Any Jetson Orin
- Docker
- Size: ~1.3GB

Gapi Server will run on other environments. Email us at support@GenAINerds.com if that's something you think is worthwhile.

Explaining the Steps:

1) On the Docker host, create working dir for persistant data
2) Download configuration files
3) Unzip
4) Pull Docker image, create container and start the process (will return console to you)

Copy and Run the Steps:

mkdir ~/gapiData && cd ~/gapiData
curl -L https://raw.githubusercontent.com/genai-nerds/Gapi/main/gapiConfigs.zip -o gapiConfigs.zip
unzip -q gapiConfigs.zip
docker run -d --name gapi --network host -v ~/gapiData:/opt/gapi/vdata genainerds/gapi:arm64 /bin/bash -c "cd /opt/gapi/bin && ./startGapi.sh"
echo "You may need to hit Enter now. Afterwards the Docker container 'gapi' should be running"

NOTE: You will need to run some Micro Services before doing anything meaningful, so please review the mini tour below but don't do any of it in the UI untill you complete the setup (instructions at the bottom)

Troubleshooting:

Keep in mind all data read or written is in ~/gapiData
Look at ~/gapiData/gapi.log to see what happened (if say the docker run command doesn't work)
gapiServerConfig.json has all the initial setup



## Problems?
~/gapiData is your friend. It has all the persistant data plus gapi.log and gapi-error.log from the application server itself.
It also has folders for special Docker Micro Service that have the respective configuration and commensurate logging

Post what the logs show as well as ~/gapiData/conf/gapi.log
```sh
docker logs gapi
```

## Run on Boot
For now you can just run "docker start gapi" in your OS startup script
