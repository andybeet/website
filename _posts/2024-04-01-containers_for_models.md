---
title: 'Run your model in a container'
date: 2024-04-01
permalink: /posts/2024/04/runmodelcontainer/
tags:
  - containers
  - docker
  - podman
---

Containers aren't new, but to the scientific community (as of writing), they kinda are! So what are containers? Well, simply put, they are a running instance of an image! Oh Right ... totally get it!

This can be quite an intimidating subject, lots of unfamiliar language, spoken by people who seem to be a little too excited about it all! So let me try to explain why you might want to run your model inside a container, what that means, and how you do it. *Disclaimer: if you are an experienced user, you might have issues with  the simplification of the following content. Sorry!*

There are several types of container products (we'll mention just three)
* [Docker](https://www.docker.com/) - probably the most well known
* [Podman](https://podman.io/) - shares many features in common with Docker but a couple of fundamental differences. Often you can just replace the `docker` command with `podman`
* [Singularity](https://apptainer.org/) - you'll find this on many scientific High Performance Computing (HPC) environments. 

### Some Definitions

* An Image is the result of building a Dockerfile or Recipe
* A Dockerfile or a Singularity Recipe is just a text file with commands to instruct Docker or Singularity, respectively, what to include in your image. It includes specifics about installation software, environment variables, files to add, etc.
* A container is the result of running your image. This will be where your model is run, inside the container, in its own little environment

### Reason why you might want to use a container

* You work on a Windows machine but need to run a model in Linux
* You need to install additional dependencies on your machine to get model to run
* Your model requires dated software which requires a specific environment to run
* Your model needs more resources than a PC and you want to move to the cloud or servers 
* You want to share your model with others
* For reproducibility and consistency

Containers provide a means to create an environment in which you can install all dependencies (just once) to get your model running. You can then transfer this environment to any platform and it will function the same.

**Note: You will need to learn a little of Linux, since most servers run Linux**

### Real example

I co-develop a multispecies ecosystem model in [ADMB](https://www.admb-project.org/). For several reasons, none of which i will discuss here, i can no longer install ADMB on my local (windows) machine. I also do not have permissions to install ADMB on any of the servers i have access to. However, `podman` is installed on the servers.

#### Create the Dockerfile

The following content should be saved in a file called `Dockerfile`

```
FROM ubuntu:18.04

# linux updates and library installs
RUN apt-get update && apt-get install -yq build-essential autoconf libnetcdf-dev libxml2-dev libproj-dev valgrind wget unzip git nano dos2unix

# pulls ADBM from github and unzips in folder ADMBcode
RUN mkdir /ADMBcode 
RUN wget https://github.com/admb-project/admb/releases/download/admb-12.2/admb-12.2-linux.zip
RUN mv admb-12.2-linux.zip /ADMBcode
RUN unzip ADMBcode/admb-12.2-linux.zip -d /ADMBcode

# pulls model repo from github into folder HYDRA
RUN mkdir /HYDRA
RUN git clone https://github.com/NOAA-EDAB/hydra_sim.git /HYDRA

# set the working directory
WORKDIR /HYDRA

# compile the model
RUN /ADMBcode/admb-12.2/admb hydra_sim.tpl

# create directory and copy files
RUN mkdir /HYDRA/mount
RUN mv /HYDRA/hydra_sim.dat /HYDRA/mount/hydra_sim.dat
RUN mv /HYDRA/hydra_sim.pin /HYDRA/mount/hydra_sim.pin

# set new working directory
WORKDIR /HYDRA/mount

# allow user to pass arguments to the container to override default
ENTRYPOINT ["sh","/HYDRA/runModel.sh"]
CMD ["hydra_sim.dat","hydra_sim.pin","1"]
```

This might seem complicated at first. However, if you step through the file it is pretty readable. A few important things to note:

* FROM ubuntu:18.04 - this pulls the Linux distribution `ubuntu` version 18:04 from Dockers repository. This is the base from which all the magic happens! After this it just a sequence of Linux commands to set up the environment for your model to run. 
* ADMB is then installed
* The model is cloned from GitHub and then compiled

Commands that are `CAPTALIZED` are Docker specific commands

#### Build the `Dockerfile`

This is the process of telling Docker to run through the commands in the `Dockerfile` to create an image, that is portable. This is achieved with one line of code

```
podman build -t model:version1 .

```

This could take a while to run. The result is an image called `model` with a tag called `version1`

#### Run the image `model:version1`

Another one liner, but a little more nuanced. Depending on how you want your model to perform, you might need to pass inputs to the model or pass output from the model. So when you run the image you will need to `mount` a path to a location on the server from a location inside the container for your data transfer. For example:

```
podman run --rm --mount "type=bind,src=/path_to_output_folder/,dst=/HYDRA/mount" model:version1

```

This will run the image `model:version1` and map the location, `HYDRA/mount`, inside the container (this is where output is sent when the model runs inside the container) to a location in your current environment, eg. the server.

### Options

Instead of using a server, as in this example, you could install `podman` or `docker desktop` on your local machine and run containers right on your desktop.


Now you have a built image why not [push it to DockerHub]({{site.baseurl}}/posts/2024/04/dockerhub/) so you can pull it to any other platform you are working on and just run your model!