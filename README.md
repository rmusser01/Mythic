--------------------------------------------------------
# Mythic
- Cross platform C2 consisting of Python3+Docker+RabbitMQ+Postgres+Web Frontend
- This is a modified fork of https://github.com/its-a-feature/Mythic
- This Version: 2.2.14
- Table of Contents
	- [Documentation](#docs)
	- [Agents & Comm Profiles](#comms)
	- [Container Configurations & PyPi Packages](#cont)
	- [Contributions](#contr)
--------------------------------------------------------

--------------------------------------------------------
#### <a name="docs">Documentation</a>
- Project Wiki: [docs.mythic-c2.net](https://docs.mythic-c2.net)
- **Browser Scripts**
--------------------------------------------------------

--------------------------------------------------------
#### <a name="comms">Agents & Comm Profiles</a>
- **Agents**
	- Mythic doesn't hold any agents itself.
		- BYOA
	- Mythic has public Agents available @ [MythicAgents](https://github.com/MythicAgents)
- **Installing Agents**
	- You need to install them yourself: 
		* `./mythic-cli install github <url> [branch name] [-f]`
	- Building Custom Agents
		* F
- Mythic Comm Profils are available @ [MythiC2Profiles](https://github.com/MythicC2Profiles) organization
	- Same as agents:
		* `sudo ./mythic-cli install github <url>`
		* Example:
			* `sudo ./mythic-cli install github https://github.com/MythicAgents/apfell`
- Building Custom Comm Profiles
	- F
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
