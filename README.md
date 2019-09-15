# PySpark
<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="">
    <img src="https://miro.medium.com/max/4914/0*Vs8O_whiQvaqSyD4.png" alt="This would have shown a cool Docker and PySpark logo :/" width="700" height="420">
  </a>

  <h3 align="center">PySpark sitting inside a Docker Container</h3>

  <p align="center">
    Combining the awesome full-stack framework with the most awesome virtualisation technology 
    <br/>
  </p>
</p>
## About the project
A small project to learn the basics of working with PySpark. 
This project barely scratched surface of what Spark can do, but it is a good start. For more information, follow this link:
https://spark.apache.org/docs/latest/sql-getting-started.html.
<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Project](#about-the-project)
  * [Built With](#built-with)
* [Getting Started](#getting-started)
  * [Virtual Env](#virtualenv)
  * [Docker](#docker)
* [Running the Container](#running-the-container)
* [Token and Password Set Up](#token-and-password-set-up)
* [Reusing the Docker Container](#Reusing-the-Docker-container)
* [Contributing](#contributing)
* [License](#license)
* [Contact](#contact)
### Built With

* [Docker](hub.docker.com)
* [PySpark Notebook](https://hub.docker.com/r/jupyter/pyspark-notebook)

## Getting Started

This project runs via a Docker container. The reason for leveraging Docker for this project was to ensure a degree of scalability for future PySpark projects where the image may need to hosted on a server, or across many in a cluster. Thereby, I wanted to avoid the old headache of a crashed server and the response of 'but it works on my machine?!'. 

### Virtual Env
The first step was to create a virtualenv:
```
$ mkvirtualenv PY_SPARK
```

This was to avoid any conflicts between libraries I would inevitably download and my global Python environment and to sandbox this current jupyter work with any other existing jupyter works.
You can be sure that you are working inside your virtualenv because the env name: 'PY_SPARK' will now appear at the beginning of the terminal line (prior to the $). 

### Docker
Here, I used a preconfigured Docker container that was installed from https://hub.docker.com/r/jupyter/pyspark-notebook
(This also has a pre-configured Dockerfile).
Note that you will need to have Docker installed prior to pulling any images, and have Docker running.

To install the above Docker image, run:
```
$ docker pull jupyter/pyspark-notebook
```
![alt text](https://github.com/Kay-Wilkinson/Py_Spark/blob/master/ReadMe_Images/docker_pull.png)

This may take some time to pull the full container, depending on internet connection and so on. 

To run the container:
```
$ docker run -it -p 8888:8888 jupyter/pyspark-notebook
```

There are other commands than can be ran in the Docker CLI, such as mapping additional ports for options (as as the Spark UI):
```
$ docker run -it --rm -p 8888:8888 -p 4040:4040 -v ~:/home/jovyan/workspace jupyter/all-spark-notebook
```
Port 4040 is the Spark UI default port setting. 

These can be added to a docker-compose.yml file that will automate running these commands for you as you 
```
$ docker up
```

## Running the Container

After running the below command:
```
$ docker run -it -p 8888:8888 jupyter/pyspark-notebook
```
You should see output similar to this:

![alt text](https://github.com/Kay-Wilkinson/Py_Spark/blob/master/ReadMe_Images/token_stuff.png)

### Token and Password Set Up
Copy and paste the token - everything after “/?token=” in the URL, into the token textbox and set a password for the Jupyter notebook server in the New Password box.
As I wanted to avoid using a token every time I spun up a Docker container to work with this Jupyter project, I decided to set a persistent password for my Jupyter notebook. 

To do so, head on over to your .jupyter env:
```
$ cd ~/.jupyter
```
You may need to replace the ~ with your username. 

If a jupyter_notebook_config.py file exists within that directory then simply run:

```
$ jupyter notebook password
```
Then enter, and re-enter, a password for your Jupyter notebooks. 

If like me, your laptop likes to play everything on 'Hard Mode', you may not have a jupyter_notebook_config.py file living in that directory. If this is the case, we just need to set one up. 
Firstly, make sure that you are still working inside of your virtualenv.
```
$ workon PY_SPARK
```
Then install juypter (or update) within that virtualenv to give us the jupyter CLI functionality. 
```
$ pip install jupyter
```
Then create the config file using (still within the `/.jupyter/` directory):
```
$ jupyter notebook --generate-config
```

The follow the previous steps above, whereby you generate a password:
```
$ jupyter notebook password
```
![alt text](https://github.com/Kay-Wilkinson/Py_Spark/blob/master/ReadMe_Images/juypter_config_setup.png)

Then go to localhost:8888 again (you may need to restart your Docker container) and login. Then, you should be able to see a UI similar to the one below:

![alt text](https://github.com/Kay-Wilkinson/Py_Spark/blob/master/ReadMe_Images/ayy_its_a_ui.png)

## Reusing the Docker Container
To access the container from a new session, you need to access it via the container id. This is a SHA hash that can be found using the below command. 
```
$ docker ps -a
```
This will list all docker containers that you have on your machine. Look for the right name e.g. `jupyter/pyspark-notebook` as there may be many containers, depending on how extensively you use Docker. 
![alt text](https://github.com/Kay-Wilkinson/Py_Spark/blob/master/ReadMe_Images/docker_ps_containers.png)
Copy the hash from the terminal output (it should be listed under CONTAINER_ID) and run the below command with the container_id appended to the command.
```
$ docker start {REPLACE_SHA_HASH_HERE}
```

## Roadmap

* Construct a docker-compose.yml file
