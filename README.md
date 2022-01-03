--------------------------------------------------------
# Mythic
- Cross platform C2 consisting of Python3+Docker+RabbitMQ+Postgres+Web Frontend
- This is a modified fork of https://github.com/its-a-feature/Mythic
- This Version: 2.3.2(maybe)
- Table of Contents
	- [Documentation](#docs)
	- ['Quick' start](#quick)
	- [Building Mythic & Associated Containers From Scratch](#building)
		- [Build Process](#basemythic)
		- [Building Mythic Payload Docker Images Manually](#manp)
	- [Agents](#agents)
		- [Installing Agents](#installa)
		- [Building Custom Agents](#cagents)
		- [Creating Custom Docker Build Environment for Agents](#cadocker)
		- [Creating a Non-docker runtime environment](#anondock)
		- [Browser Scripts](#abrowsers)
	- [Creating Custom Translation Containers](#translate)
		- [101](#translate101)
		- [Building One](#translatebuild)
		- [Installing The Newly Created Translation container](#translateinstall)
	- [C2 Communication Profiles](#comms)
		- [Installing C2 Communication Profiles](#installc)
		- [Building Custom C2 Communication Profiles](#ccomm)
		- [Creating Custom Docker Build Environment for C2 Comm Profiles](#ccdocker)
		- [Creating a Non-docker runtime environment](#cnondock)
	- [Customizing Mythic](#custom)
	- [Container Configurations & PyPi Packages](#cont)
	- [Contributions](#contr)
--------------------------------------------------------

--------------------------------------------------------
#### <a name="docs">Documentation</a>
- This page: https://github.com/rmusser01/Mythic/tree/master
- Official Project: https://github.com/its-a-feature/Mythic
- **Links**
	- Core project: https://github.com/its-a-feature/Mythic
	- Project Wiki: [docs.mythic-c2.net](https://docs.mythic-c2.net)
	- Git repo of documentation in markdown: https://github.com/MythicMeta/Documentation
	- Source code for UI: https://github.com/MythicMeta/MythicReactUI
	- Source for 'Meta' files(Dockerfiles, mythic-cli binary, etc): https://github.com/MythicMeta
	- Creating an Agent: https://docs.mythic-c2.net/customizing/payload-type-development
- **Browser Scripts**
	- F
- **Phrases & Vocab**
	* 'Payload' == Agent.
	* 
--------------------------------------------------------



--------------------------------------------------------
#### <a name="quick">Quick Start</a>
- **Install/Configure Mythic**
	- **Installing**
		0. [Official Docs for Installing](#https://docs.mythic-c2.net/installation)
		0. Install Docker + Docker Compose
			* FIXME
		1. Create your working directory for Mythic, `cd` into it, and run:
			* `git clone https://github.com/its-a-feature/Mythic`
		2. Configure Mythic.
		 	* FIXME
		4. Start Mythic:
			* `sudo ./mythic-cli mythic start`
		5. Find out the Administrator password:
			* `sudo ./mythic-cli config get MYTHIC_ADMIN_PASSWORD`
		6. Access Mythic through a browser at:
			* `0.0.0.0:7443`
		7. Login.
			* Username: `mythic_admin`

	- **Configuration**
- **Install/Configure Agents**
	- **Installing**
		- Publicly available:
			1. 
			2.
			3.
			4.
			5.
			6.
			7. 
	- **Configuration**
- **Install/Configure C2 Profiles**
	- **Installing**
		- Publicly available:
			1. HTTP
			2. Dynamic HTTP
			3. DNS
			4. Websockets
	- **Configuration**
- **Getting Started With An Operation**
	0. Assuming you've got Mythic setup, running, and have an Agent installed
	1. Setup a C2 Profile.
		1. `Global Configuration` -> `C2 Profiles`
		2. Select `Configure` -> `Configure Server` next to whichever profile you would like.
	2. Configure an Agent
	3. Deploy Agent
	4. Profit!
--------------------------------------------------------



--------------------------------------------------------
#### <a name="building"></a>Building Mythic C2 From Scratch
- Pre-requisites
	* We want to build mythic from source, completely.
		* This is so we can verify and validate every package pulled in, and what every piece of the framework does.
			* Better understanding == Better usage
	* Build Environment
- **Build Process**<a name="basemythic"></a>
	1. First we build `mythic-cli`
		- Building 'mythic-cli'
			* `git clone https://github.com/MythicMeta/Mythic_CLI`
			* `cd Mythic_CLI`
			* `go build -ldflags="-s -w" -o mythic-cli main.go`
			* `upx --brute mythic-cli`
				* Compress the binary size
	2. Then we build each container used for Mythic.
		1. First we need docker.
			* Docker install instructions here.
		2. Now that we have Docker installed, lets work on building each docker image.
			- Crafting Custom Dockerfiles & Containers (because sometimes you just want that farm to table taste)
				0. Script to do it for some of the below dockerfiles: https://github.com/MythicMeta/Mythic_Docker_Templates/blob/master/Docker_Mythic_Services_base_files/create_base_images.sh
				0. Repo containing all Dockerfiles used:
					* https://github.com/MythicMeta/Mythic_Docker_Templates
				1. hasura-docker
					* Base image is: `hasura/graphql-engine:v2.0.3.cli-migrations-v2`
						* https://hub.docker.com/layers/hasura/graphql-engine/v2.0.3.cli-migrations-v3/images/sha256-b54b8f3704538a25dda819cd806e9194c83cedb251c85e11e9139b17ff236824?context=explore
				2. mythic-docker
					* `FROM itsafeaturemythic/mythic_server:0.0.4` -> `From python:3.8-buster`
						* Mythic: https://github.com/MythicMeta/Mythic_Docker_Templates/blob/master/Docker_Mythic_Services_base_files/mythic_server
						* Python: https://hub.docker.com/layers/python/library/python/3.8-buster/images/sha256-5b5fd3c2582c3398e095fb0b0dc16fff20ddb27a40a8d4d67da673635c0c8821?context=explore
				3. mythic-react-docker
					* `From itsafeaturemythic/mythic_react:0.0.4`
						* Mythic: https://github.com/its-a-feature/Mythic/blob/master/mythic-react-docker/Dockerfile
						* dockerfile 0.4: https://hub.docker.com/layers/itsafeaturemythic/mythic_react/0.0.4/images/sha256-3822f9a9d0c146bf4a29389f697cae0bc6c5390fb401f79041c317d77c4480cf?context=explore
						* dockerfile 0.6: https://hub.docker.com/layers/itsafeaturemythic/mythic_react/0.0.6/images/sha256-2d3aa44a142f180e216804b7b810d4cf2975eef7595072a961daa2007ee021b1?context=explore
				4. nginx-docker
					* `FROM nginx:1.21.4-alpine`
						* Mythic: https://github.com/its-a-feature/Mythic/tree/master/nginx-docker
						* Nginx: https://hub.docker.com/layers/nginx/library/nginx/1.21.4-alpine/images/sha256-f51b557cbb5e8dfd8c5e416ae74b58fe823efe52d9f9fed3f229521844a509e2?context=explore
				5. postgres-docker
					* `From itsafeaturemythic/mythic_postgres:0.0.2` -> `From postgres:13-alpine`
						* Mythic: https://github.com/MythicMeta/Mythic_Docker_Templates/blob/master/Docker_Mythic_Services_base_files/mythic_postgres
						* Postgres: https://hub.docker.com/layers/postgres/library/postgres/13-alpine/images/sha256-baec95683e6db5dbcfe26b73e0cd137412e3557f62128a9c368bdc45268cacb8?context=explore
				6. rabbitmq-docker
					* `From itsafeaturemythic/mythic_rabbitmq:0.0.2` -> `FROM rabbitmq:3-alpine`
						* Mythic: https://github.com/MythicMeta/Mythic_Docker_Templates/blob/master/Docker_Mythic_Services_base_files/mythic_rabbitmq
						* RabbitMQ: https://hub.docker.com/layers/rabbitmq/library/rabbitmq/3-alpine/images/sha256-c7c87d795e38cd1c3a009e3e1a80f8e8e7a649e4d8cfaa0e099013fad5cf8ebf?context=explore
				7. redis-docker
					* `FROM redis:6-alpine`
						* Mythic: https://github.com/its-a-feature/Mythic/blob/master/redis-docker/Dockerfile
						* Redis: https://hub.docker.com/layers/redis/library/redis/6-alpine/images/sha256-71be06832923622a7f94c205e6580e55c50d6710eea26aac18444a695352fe78?context=explore
	3. Now that you've done something with the above, you should have a fully customized and 'farm-to-table' grown and produced instance of Mythic. 
		* "Yay for software bill of materials!"
- **Building Mythic Payload Docker Images Manually**<a name="manp"></a>
	* **Pre-Requisites**
		- **Payload Type Containers**
			* Docker containers for building agents
			* Can be wherever you want, as long as comms with Mythic server possible.
			* Container manages all browser scripts for agent.
	0. Start with copying the folder `Mythic/Example_Payload_Type`, into the `Mythic/Payload_Types` folder, and renaming the newly copied `Example_Payload_Type` folder into `<Your_Agent_Name_Here>`
		- Link to github: https://github.com/its-a-feature/Mythic/tree/master/Example_Payload_Type
	1. Building the Dockerfile Container
		0. Dockerfile consists of `from itsafeaturemythic/csharp_payload:0.0.11` with the comment `# pull in the appropriate language's payload container from itsafeaturemythic on dockerhub`
		1. Taking a look at `itsafeaturemythic on dockerhub`, specifically `itsafeaturemythic/csharp_payload`, 
			* https://hub.docker.com/r/itsafeaturemythic/csharp_payload/tags
			* We see the latest version being `0.0.17`: https://hub.docker.com/layers/itsafeaturemythic/csharp_payload/0.0.17/images/sha256-8acde5a5c1787f9d803998d93867e8fbf779dfb5080eb4114fe182584b45a89f?context=explore
			* We can see all the commands used to build the image here: https://hub.docker.com/layers/amd64/mono/6.12-slim/images/sha256-13cd48057015fbe9d609c2346637e4ba9bc4f63d74da10d1387c779b07ce7d2a?context=explore
			* Going further, the first file copied into the instance is FIXME (Debian Buster?)
			* The final file copied into the dockerfile is FIXME (??)
		2. With all this known, we can modify or replicate it ourselves, and create our own appropriate Dockerfile.
- **Building Mythic Payloads & associated Docker Images using Jenkins**
	0.
	1.
- **Building Mythic Translator Docker Images Manually**
	0.
	1.
- **Building Mythic Translator Docker Images using Jenkins**
	0.
	1.
- **Building Mythic C2 Profile Docker Images Manually**
	0.
	1.
- **Building Mythic C2 Profile Docker Images using Jenkins**
	0.
	1.
- **Building Mythic & all of its associated Docker Images using Jenkins**
	0. 
	1.
	2.
--------------------------------------------------------






--------------------------------------------------------
#### <a name="custom"></a> Customizing Mythic
- Scripting It
	* [Scripting](https://docs.mythic-c2.net/scripting)
	* [Mythic_Scripting](https://github.com/MythicMeta/Mythic_Scripting)
--------------------------------------------------------






--------------------------------------------------------
#### <a name="agents">Agents</a>
- **Agents**
	- Mythic doesn't hold any agents itself.
		- BYOA
	- Mythic has public Agents available @ [MythicAgents](https://github.com/MythicAgents)
	- Agents handle all linked browser scripts, so if you want special functionality in the UI, remmember those Browser Scripts...
- **Installing Agents**<a name="installa"></a>
	- You need to install them yourself: 
		1. `./mythic-cli install github <url> [branch name] [-f]`
		2. 
		3. 
- **Building Custom Agents**<a name="cagents"></a>
	- **Background info**
		* By default, agents are built using customized docker containers.
		* Base image: `From itsafeaturemythic/python38_payload:0.0.7`
			* See 
			* FIXME - add link to container
		* 
	- **Building**
		* [Official Documentation Page](https://docs.mythic-c2.net/payload-types)
		* [Mythic_PayloadType_Container](https://github.com/MythicMeta/Mythic_PayloadType_Container)
			* "The mythic_payloadtype_container package creates an easy way to get everything set up in a new PayloadType container for a Mythic supported Payload Type."
		- **High-Level Process**
			0. Clone/copy the `Example_Payload_Type` folder, from https://github.com/its-a-feature/Mythic/tree/master/Example_Payload_Type -> To the `Payload_Type` folder in the `Mythic` folder.
				* FIXME - add instructions for replicating structure for private git repos.
				* Also be sure to rename the copied folder, `Example_Payload_Type` to whatever your Agent's name is.
			1. You should now have a folder `<Agent_Name>` under the `Mythic/Payload_Types` folder.
				- The structure should look like the following:
					0. `agent_code`
						* All files used for building the agent stored here.
					1. `mythic`
						- `agent_functions`
							- `Builder.py`
								* Commands used to build the actual Agent inside the docker container.
								* https://docs.mythic-c2.net/customizing/payload-type-development/payload-type-info
								* If you are going to be using a Translator Container, that needs to be defined here, under:
									* `translation_container "<Translator_Container_Name>"`in your agent's builder.py file (where you define all the attributes of your payload type) add in the line translation_container = "my_translator"
								* Encryption is also enabled here:
									* `mythic_encrypts = <True/False>`
							- `<Command_Name>.py`
								* Any other files for agent commands
						- `browser_scripts`
							* Associated browser scripst for enhanced functionality.
						- `mythic_service.py`
							* Creates mythic service: `mythic_service.start_service_and_heartbeat()`
						- `payload_service.sh`
							* Runs `python3.8 mythic_service.py`
						- `rabbitmq_config.json`
							* Configuration file for rabbitmq instance communications
					2. `Dockerfile`
						* Dockerfile for defining/building Container for Agent creation/publishing.
			2. Open up `Mythic/Payload_Types/<Your_Agent_Name>/Builder.py`
				* We want to modify this script to fit our new payload.
				* Most of the fields are self-explanatory, some that may not be at first glance:
					* `wrapper`	
						* "Payload agnostic components payloads can hook into" - mentions MSBuild as an example
						* "Wrapping mechanism to help deliver the payload"
					* `note`
						* Note displayed to operator when selecting payload.
					* `supports_dynamic_loading`
						* Defines whether you can allow for modular command stamping into your Agent.
					* `build_parameters`
						* Any sort of params for building
					* `c2_profiles`
						* _MUST SPECIFICALLY DEFINE SUPPORTED COMM METHODS HERE_
					* `async def build(self) -> BuildResponse:` line
						* Simple function to return status of build.
			3. Decide on what type of communications will be used between your agent and the Mythic server.
				* Mythic uses JSON serverside for input, and does support having 'tranlsation containers' to perform conversions between various data types before processing.
				* Callback structure can be seen at FIXME
				* After deciding on the Communication method, you will then need to ensure two things: 
					1. that your desired communication method exists as a C2 Communication profile, and 
					2. Your agent supports the desired communication method.
			4. At this point, you should have your communication method chosen, and a blank template folder renamed to your desired agent name. 
				- You now have 3(4) parallel tasks:
					1. Decide on and implement your C2 Communication Method.
						* Something to consider is that your data needs to be JSON when it hits the main Mythic server.
							* You can do processing locally on the agent, or through a `Translation Container` which then feeds into a C2 Communication profile.
							* Both have pros/cons, but is something to consider heavily.
							* [Translation Containers Documentation](https://docs.mythic-c2.net/customizing/payload-type-development/translation-containers)
								* If you are going to be using a Translator Container, that needs to be defined in the `builder.py` script, under:
									* `translation_container "<Translator_Container_Name>"`in your agent's builder.py file (where you define all the attributes of your payload type) add in the line translation_container = "my_translator"
								* You will also need to set whether encryption enabled there too:
									* `mythic_encrypts = <True/False>`
					2. Agent Functionality/Commands supported
						* Mythic tracks this through `<command_name>.py` scripts in the `Mythic/Payload_Types/<Your_Agents_Name>/mythic/agent_functions/` folder.
						* Scripts placed here help Mythic understand and make available commands from/for your agent.
					3. Agent Code itself
					4. Task Handling
						* How will your agent handle tasking from Mythic?
	- **Payload Container Syncing**
		* [Container Syncing](https://docs.mythic-c2.net/customizing/payload-type-development/container-syncing)
		* On startup, your payload container should perform two immediate actions:
			1. Connect to RabbitMQ instance defined.
			2. Send a Heartbeat to Mythic;
				* If Mythic Server doesn't have the agent in record, it sends a sync message
					* "This data is simply a JSON representation of everything about your payload - information about the payload type, all the commands, build parameters, command parameters, browser scripts, etc."
				* Times this will happen:
					* When a payload container is started
					* If a container issues a heartbeat message to Mythic, and Mythic doesn't have the agent on record.
					* If a C2 profile syncs with Mythic.
					* When any payload container syncs, it triggers a reload of all 'Wrapper' payload types.
				* Current Versions:
					- 10 (Mythic 2.2.15)
						* `mythic_payloadtype_container==0.0.47`
						* `itsafeaturemythic/python38_payload==0.0.8`
						* `itsafeaturemythic/leviathan_payload==0.0.7`
						* `itsafeaturemythic/csharp_payload==0.0.15`
						* `itsafeaturemythic/xgolang_payload==0.0.13`
	- **Payload Development**
		* [Payload Type Info](https://docs.mythic-c2.net/customizing/payload-type-development/payload-type-info)
		* 
	- **Payload <-> Mythic Commands**
		* [Commands](https://docs.mythic-c2.net/customizing/payload-type-development/commands)
		- "What do Commands track?"
			* 
		- 
	- **Tasking**
		* [Create_Tasking](https://docs.mythic-c2.net/customizing/payload-type-development/create_tasking)
		* [Sub-Tasking/Task Callbacks](https://docs.mythic-c2.net/customizing/payload-type-development/sub-tasking-task-callbacks)
	- **Mythic Hooking**
		* 
- **Doing it**<a name="devagent"></a>
	- Assuming you're starting from the above steps, under 'Building', we are now going to look at building a _very_ basic Agent.
		* Basically we're going to replicate what was demonstrated in the SOCON2020 video. But in 2022. With other langs than just PowerShell.
	- **X Lang**
		- We're going to create a basic PoC agent in X, which will register as a callback, perform a local file listing, and list the name of the current user.
		0. 
- **Creating Custom Docker Build Environment for Agents**<a name="cadocker"></a>
	- **Making One**
		0. Create a new folder named the same as your agent under `Mythic/Payload_Types/`.
		1. Create a `Dockerfile` inside said folder, `Mythic/Payload_Types/<agent_name>/Dockerfile`
			* We'll use `itsafeaturemythic/csharp_payload:0.0.13` for this example
		2. Create the agent's source code, command structure and c2 comms profile settings.
			* `Mythic/Payload_Types/<agent_name>/agent_code/<files_here>`
				* "You can have any folder structure or files you want here."?
			* API Information for mythic <-> client comms: `Mythic/Payload_Types/<agent_name>/mythic/`
			* All agent commands: `Mythic/Payload_Types/<agent_name>/mythic/agent_functions`
		3. If and when building agents on remote servers, i.e. ones where mythics binary isn't being ran:
			* "If your container/service is running on a different host than the main Mythic instance, then you need to make sure the rabbitmq_password is shared over to your agent as well. By default, this is a randomized value stored in the Mythic/.env file and shared across containers, but you will need to manually share this over with your agent either via an environment variable (MYTHIC_RABBITMQ_PASSWORD or by editing the rabbitmq_password field in your rabbitmq_config.json file."
		4. After performing the above, you'll need to add the newly created agent to Mythic, and can do so with the following command:
			* `mythic-cli payload add <payload name>`
		5. To then start your payload container:
			* `mythic-cli payload start <payload name>`
			* "Your container should pull down all the necessary files, run the necessary scripts, and finally start. If it started successfully, you should see it listed in the payload types section when you run sudo `./mythic-cli status.`"
			* "If you go back in the web UI at this point, you should see the red light next to your payload type change to green to indicate that it's now getting heartbeats. If it's not, then something went wrong along the way. You can use the `sudo ./mythic-cli logs payload_type_name` to see the output from the container to potentially troubleshoot."
			* "The containers will automatically sync all of their information with the Mythic server when they start, so the first time the Mythic server gets a message from a container it doesn't know about, it'll ask to sync. Similarly, as you do development and restart your Payload Type container, updates will automatically get synced to the main UI."
			* Enter the docker container:
				* `sudo docker exec -it {payload type name} /bin/bash`
- **Creating a Non-docker runtime environment**<a name="anondock"></a>
	1. Install python 3.8+
	2. `pip3 install aio_pika` for RabbitMQ comms
	3. `pip3 install mythic-payloadtype-container` - "this has all of the definitions and functions for the container to sync with Mythic and issue RPC commands"
	4. Recreate the path structure of the `PayloadTypes/<agent_name>` under a designated parent folder
		* "copy everything from the `Example_Payload_Type`"
	5. You should now have replcated the `Payload_Types/<agent_name>` folder under your designated parent folder.
	6. Edit `rabbitmq_config.json` 
		* `host` - IP of the main Mythic install
		* `name` - Absolute path to Agent/Payload folder.
	7. Set `PYTHONPATH` variable, adding your `/<Agent_Name>` and `/<Agent_Name>/mythic`
	8. Run `python3 mythic_service.py`
		* If everything was done properly, and the Agent type is registered in Mythic already, you should see a green light showing its status as ready.
- **Translation Containers**<a name="translationc"></a>
	- See [Translation Containers](#translate)
- **Browser Scripts**<a name="abrowsers"></a>
	- **101**
		* 
--------------------------------------------------------






--------------------------------------------------------
#### <a name="translate">Creating Custom Translation Containers</a>
- **101**<a name="translate101"></a>
	* Mythic Translation Containers are a means of avoiding dealing with JSON on your agent. 
	* You can instead shuffle data from your agent to a '`Translation Container`' and have that process and decompose your data into a JSON object that mythic can then process normally.
	* This means that you can use whatever fancy transport protocol you'd like to use, and not have to worry about funky JSON transformations on the agent-side.
	* Link to template for building one:
		* [Example_Translator](https://github.com/its-a-feature/Mythic/tree/master/Example_Translator)
- **Building One**<a name="translatebuild"></a>
	0. Clone/copy the `Example_Translator` folder, [here](https://github.com/its-a-feature/Mythic/tree/master/Example_Translator) -> To the `Payload_Type` folder in the `Mythic` folder.
		* FIXME - add instructions for replicating structure for private git repos.
		* Also be sure to rename the copied folder, `Example_Translator` to whatever your Translator's name is.
	1. You should now have a folder `<Translator_Name>` under the `Mythic/Payload_Types` folder.
		- The structure should look like the following:
			1. `mythic`
				- `c2_functions`
					- `__init__.py`
						* Empty.
					- `C2_RPC_functions.py`
						* Provides the base-functions for transforming data to/from JSON for Mythic's consumption.
				- `__init__.py`
					* Empty.
				- `c2_service.sh`
					* Runs `python3.8 mythic_service.py`
				- `mythic_service.py`
					* Creates mythic service: `mythic_service.start_service_and_heartbeat()`
				- `rabbitmq_config.json`
					* Configuration file for rabbitmq instance communications
			2. `Dockerfile`
				* Dockerfile for defining/building Container for Translater.
	2. FIXME
- **Installing The Newly Created Translation container**<a name="translateinstall"></a>
	0. Copy the `<Translator_Container_Name>` into the `Mythic/Payload_Types/` folder.
	1. Then, add the newly created container to Mythic:
		* `sudo ./mythic-cli payload add <Translator_Container_Name>`
	2. Next we need to start it:
		* `sudo ./mythic-cli payload start my_translator`
	3. Finally, restarting your agent so that it recognizes the new Translator Container:
		* `sudo ./mythic-cli payload start my_agent`
--------------------------------------------------------






--------------------------------------------------------
#### <a name="comms">Creating Custom C2 Communication Profiles</a>
- Mythic Comm Profiles are available @ [MythiC2Profiles](https://github.com/MythicC2Profiles) organization
	- Same as agents:
		* `sudo ./mythic-cli install github <url>`
		* Example:
			* `sudo ./mythic-cli install github https://github.com/MythicAgents/apfell`
- Building Custom Comm Profiles
	- F
		* https://github.com/MythicMeta/Mythic_C2_Container
	- Custom C2 Translator
		* https://github.com/MythicMeta/Mythic_Translator_Container
--------------------------------------------------------





--------------------------------------------------------
#### <a name="cont">Container Configurations & PyPi Packages</a>
- Associated Files Located @ [MythicMeta](https://github.com/MythicMeta)
	* Dockerfile configurations for all Docker images uploaded to DockerHub
	* PyPi source code for all packages uploaded to PyPi
	* Scripting source code
- **Containers(Docker)**
	- Docker & Docker-compose is used for infra setup.
	- All Mythic-related docker images hosted at dockerhub:
		* [itsafeaturemythic](https://hub.docker.com/search?q=itsafeaturemythic&type=image)
- **PyPi Packages**
	- Mythic uses custom PyPi packages.
		* "Additionally, Mythic uses a number of custom PyPi packages to help control and sync information between all of the containers as well as providing an easy way to script access to the server."
	- **Current Container PyPi Package requirements**
		* Supported payload types must have the `mythic_payloadtype_container` PyPi package of 0.0.43.  
			* The Payload Type container reports this as version 7.  
		* Supported c2 profiles must have the `mythic_c2_container` PyPi package of 0.0.22.  
			* The C2 Profile container reports this as version 3.  
		* Supported translation containers must have the `mythic_translator_containter` PyPi package of 0.0.10.
			* The Translator container reports this as version 3.  
--------------------------------------------------------










--------------------------------------------------------
## <a name="contr">Contributions</a>

A bunch of people have suffered through bug reports, changes, and fixes to help make this project better. Thank you!

The following people have contributed a lot to the project. As you see their handles throughout the project on Payload Types and C2 Profiles, be sure to reach out to them for help and contributions:
- [@djhohnstein](https://twitter.com/djhohnstein)
- [@xorrior](https://twitter.com/xorrior)
- [@Airzero24](https://twitter.com/airzero24)

## Liability

This is an open source project meant to be used with authorization to assess the security posture and for research purposes.
