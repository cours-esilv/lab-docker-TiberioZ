# Lab Docker

## Instructions
Answers to the questions in this statement should be submitted as files named according to the question number in your repository, in the `answers` folder. 
Each answer will be the subject of a new file in your repository.
> Questions requiring an answer in the form of a file will be tagged with the following icon: ⚠️.  These files are corrected automatically by Github Autograde.

> Commits are not optional. Commits are not optional **Commit as soon as the statement asks you to**.

The TP must be submitted individually and will be based on the answers in the files in the `answers` folder, as well as on the code in your personal repositories.

## Reminders
### Docker
* Concepts
  * Build
  * Ship (Push)
  * Run
  * Pull
* [Docker doc reference](https://docs.docker.com/reference/)
  * [Dockerfile](https://docs.docker.com/engine/reference/builder/)
  * [docker-compose.yml](https://docs.docker.com/compose/compose-file/)

## 0 : Project description
Throughout this tutorial, we'll be basing ourselves on the project located in the app directory of this repository. This application consists of front-end html that calls a REST API (back-end) that returns information.

The application returns various items of information:
* the time of the call
* the environment in which the backend was deployed (environment name)
* the URL called
* the backend hostname

The backend service can be requested on :
* a path starting with /get/{something} (e.g. http://localhost:8080/get/toto)
* or on the /write/{something_to_write} path (eg: http://localhost:8081/write/something_to_write)

A call to the /write path in the backend will write the argument passed as a parameter to a log file located in the back container.

## 1: Docker
This first part must be done individually.

### 1.0: Installing Docker
To carry out the following tutorial, you will need Docker. To do this, I suggest you use the GitPod service, which provides machines with a free tier of 50 hours per month.
To set up your account:
* Go to [the Gitpod site](https://www.gitpod.io/) and log in with your GitHub account (the same as the one you'll be using to do the practical work).
* Then open a new tab and type the following url, modifying it to `https://gitpod.io/#<URL_OF_YOUR_REPO>`.
* Your workspace will take a few seconds to be created. When you have access to it, you can run the `docker ps` command to check that everything is working correctly

> **Warning**
> If you prefer, you can install Docker directly on your computer and do all the tp on it. To do this, follow [the official doc](https://docs.docker.com/get-docker/)
> However, if you're not sure, you're better off using the Gitpod solution.

### 1.1: Understanding the app
Inspect the files in the **app** directory to understand the structure of the project.

### 1.2: Let's Build it
In the app/back directory, create a **Dockerfile** capable of building a python image containing our back code.

Help:
* Consult the [Dockerfile reference documentation](https://docs.docker.com/engine/reference/builder/)
* Use a python image in Docker Hub [Docker Hub](https://hub.docker.com/)
* Python uses libraries that must be installed on a host before the program can be started. To install these libraries, run the following command (in the Dockerfile)
```bash
pip install -r requirements.txt
```
* Once the Python libraries have been installed, the command to run in the Docker image is
```bash
python3 -m flask run --host=0.0.0.0
```
* the **WORKDIR** command may help

### 1.3 : An inconclusive first run
Launch a container from the Docker image you have just created.

> ⚠️ **ANSWER**: Create a file called `1.3` containing the commands you used to launch the container.

### 1.4: Accessing the service
Try to access the application at the URL indicated in the logs (:warning:) Why do you think this address is not responding? 

Restart the container and open the ports required to access the service.
> ⚠️ **ANSWER**: Create a file called `1.4` containing the commands you used to start the container with the ports open.

### 1.5 : Env var
Try to access the application at the URL indicated by the logs. Go back to your terminal and you'll be presented with a nice error stacktrace. This error occurs because the program expects to find the environment variable **CURRENT_ENVIRONMENT** present in the container.

Restart the container and add the missing environment variable so that the service responds correctly (you can set the value you prefer).

> ⚠️ **ANSWER**: Create a file called `1.5` containing the command you used to start the container with the environment variable and the open ports.

The application starts up correctly and you can access it by following the URL displayed in the logs.

### 1.6: Let's Ship it
In order to share your fantastic new creation with the world, we're going to push the image to the official Docker registry called Docker Hub (or Docker Store)

To do this: 
* Create an account on [Docker Hub](https://hub.docker.com/)
* Create a new repository attached to your account
* Then push your newly created image to your repository.

Your image cannot be pushed as it is (:warning:) Why do you think this is? Change the name of your image and push it back onto your Docker Hub repository.

> ⚠️ **ANSWER**: Create a file called `1.6` containing the commands you used to push your image to Docker Hub.

### 1.7: Clean slate
Now that your image is shared with the world, you can pull it from any machine on which Docker is installed.

Delete all your images created since the start of the tp on your machine.

> ⚠️ **ANSWER**: Create a file called `1.7` containing the commands you used to delete all the images created since the start of the tp.

### 1.8 : Let's Run it
Now relaunch a container based on the image you've just pushed into your Docker Hub repository. (:warning:) What happens before the image starts?

Your container works as expected but if you close your terminal, it stops. Restart the container in detached mode and check that the container is accessible via its web interface.

> ⚠️ **ANSWER**: Create a file called `1.8` containing the commands you used to launch the container in detached mode.

### 1.9 : Naming is important
Your container seems to have started. In detached mode, which command will allow you to check that the container has started? (:warning:) What is the name of your container?

> ⚠️ **ANSWER**: Create a file called `1.9.1` containing the commands you used to check the state of the container.

Restart your container by renaming it to something meaningful.

> ⚠️ **ANSWER**: Create a file called `1.9.2` containing the commands you used to name your container.

### 1.10 : Go inside
Your container back now started, open an interactive session to observe the files inside the container. After opening an interactive session, run the following command to retrieve information about the OS used in the container:
```bash
cat /etc/*release
```
What OS is being used in the container?

> ⚠️ **ANSWER**: Create a file called `1.10` containing the commands you used to start an interactive session.

### 1.11: Do it again (as Steely would say [BP])
Go to the app/front directory and carry out all the steps in parts 1.2 to 1.10 to create the front image of our application.

> warning: **WARNING**: Do not create an answer file for the front end. The aim of this question is to have a functional front-end container ready to integrate with the back-end of our application.

> warning: **WARNING**: The frontend is a simple html page. You therefore need to start from a different base image.

Help: 
* You need to use a different base image (any web server will do).
* Be careful, you must use an entrypoint in the front directory. See https://docs.docker.com/engine/reference/builder/#exec-form-entrypoint-example
* The environment variables to set are : **WS_BACK_URL**
* If both services are started, be careful about exposing ports so as not to conflict.

> Don't forget to push your image to your docker hub registry.

### 1.12: It's better with two people
Launch your back container at the same time as your front container and modify the front environment variables to make it point to the back.

> ⚠️ **ANSWER**: Create a file called `1.12` containing the commands you used to start a front container pointing to the back container.

### 1.13 : Testing write urls
Use the `/write` path of the backend URL to write to the log file of the back container.
Check the contents of the log file by connecting to the container back in interactive mode.

### 1.14: Clean-up
Stop all the containers and delete all the local images you have created so far.

> ⚠️ **ANSWER**: Create a file called `1.14` containing the commands you used to stop all containers.

#2: Docker Compose
In this part, we're going to use docker-compose to automate the deployment of the containers we launched in part 1 of the tp.

### 2.0: Download docker-compose
docker-compose is available in Gitpod.

However, if you have installed Docker on your workstation and want to install docker-compose too, go to [the official site](https://docs.docker.com/compose/install/) and follow the official instructions.

> Caution: Check that you have stopped all your containers before starting this part.

### 2.1: Creating the docker compose
At the root of the app directory, create a docker-compose.yml file containing the following code:
```yaml
version: '3'
services:
  my-service:
    image: my-image
```

Edit the file to point to your back image and rename the service as you wish. Then start the back service using the **docker-compose** commands. 

> Documentation available on [the official website](https://docs.docker.com/compose/reference/)

> ⚠️ **ANSWER**: Create a file called `2.1` containing the commands you used to start the back service via docker-compose. 

### 2.2: Ports
Modify the docker-compose.yml file to add a mount on the accessible port of the back service.

Restart the container and test the solution.

### 2.3 : Env var
Modify the docker-compose.yml file to add the environment variable required to use the back service.

Restart the container and test the solution.

### 2.4: Two is better
Modify the docker-compose.yml file to add the front-end service. Don't forget to fill in the ports and environment variables.

Restart the container and test the solution.

### 2.5: Unhook it!
Restart all the services in detached mode.

> ⚠️ **ANSWER**: Create a file called `2.6.1` containing the commands you used to start all the services in detached mode.

View the logs of the various services at the same time.

> ⚠️ **ANSWER**: Create a file called `2.6.2` containing the commands you used to view all the logs for the various services at the same time.

### 2.6 : Persistence
The services are now launched correctly via docker-compose. However, each time they are restarted, the data is flushed.

Modify the docker-compose.yml to add a volume mount in order to persist the data on the disk between restarts.

> Caution: After completing this step, comment out the lines relating to the volume mount in the docker-compose.yml.

### 2.7 : Upgrade
At any time when you have launched your services in detached mode, you can reload a new version by executing the line below again:
```bash
docker-compose up -d
```

### 2.8: Scaling space
Services must be started in detached mode. Try scaling the back webservice by tripling the number of back containers started.

> ⚠️ **ANSWER**: Create a file called `2.8` containing the commands you have used to scale the webservice back.

Check that the hostnames change in the UI.

And that's it!
