# Current weather app

![diagram](https://github.com/aakkiiff/CurrentWeatherApp/blob/master/Diagram.jpg?raw=true)
**everything is pretty self explanatory,just add your own apikey at docker compose file from the website:[rapidapi.com](https://rapidapi.com/)

and add the api 

> rapidapi.com -> weatherapi.com ->apikey

 **
 get the api key and add to the compose file.

## docker compose

 - docker compose up --build

# CI/CD the GitOps way
- **jenkins will be used as CI**
- **2 GitHub repo will be used, one for app src code and another for config files**
![diagram](https://github.com/aakkiiff/CurrentWeatherApp/blob/master/Diagram2.jpg?raw=true)
## steps:
1. Make 2 repository one for app source code another for config files
2. or, clone the repo
3. spin up aws ec2,install jenkins and docker in it.
4. install a plugin "Generic Web hook Trigger"
5. Make a pipeline project
	
	- [x] Generic Webhook Trigger
	- add token = "anythingyouwant"
	- pipeline definition = pipeline script for Scm.
6. now go to your github projects's webhook configuration  
	

	    http://JENKINSIPADRESS/generic-webhook-trigger/invoke?token="YOUR TOKEN"

	 - Content type=APP/JSON
	 - only send push event 
7. make a 2nd pipeline for tagging and name it,use this name in the first jenkinsfile to make itself trigger once the 1st pipeline is done.(more details of this part from 8th step)
`https://github.com/aakkiiff/CurrentWeatherApp_Config`
	 
**NOW YOUR FIRST JENKINS PIPELINE IS READY,PUSH YOUR CODE TO THE REPO AND SEE THE JOB RUNNING,BUILDING AND PUSHING DOCKERFILE TO DOCKERHUB**

8. configuring the second pipeline:
	

	 - [x] This project is parameterized
	 - name = IMAGE_TAG ,same name should be used on the last command of 1st pipeline `build job: 'currentweatherapp_config', parameters: [string(name: 'IMAGE_TAG', value: env.IMAGE_TAG)]`
	 - env.IMAGE_TAG is keeping the value of tag from the pipeline and storing it into `name: 'IMAGE_TAG'`
	
9. pipeline definition = pipeline script for Scm.

10. all set,first job will trigger the 2nd one and 2nd one will take the tag number and tag the deployment files ,and push the changes  to git repo.
