---
title: 'Use DockerHub to store and retrieve your containers'
date: 2024-04-15
permalink: /posts/2024/04/dockerhub/
tags:
  - containers
  - docker
  - podman
---

So you have just built your first docker image (or podman image, but more about that in a minute). And it runs! And you are excited! So you start to tell people how cool you are, that you can run "stuff" inside a container. But then are quickly brought back down to earth when you realize that you don't know how move this image to other platforms, like the cloud. It resides "somewhere" on your local machine, in a location that isn't obvious.

Of course you could go through the steps of building the image again on the other platform, but that just seems uncool!

Enter [DockerHub](https://hub.docker.com/) ... a repository for container images. Think of it like GitHub, but for docker images. Once your images are on DockerHub, you can pull them to any platform you are working on, with minimal effort. You never have to rebuild them again!

But first, you'll need to push them to DockerHub. 

Step one: sign up for a free account.
Step two: log in to your account (from the command line)
Step three: create a tag for your image
Step four: push the image

```
docker login docker.io
docker tag myCoolImage:tag username/myCoolImage:tag
docker push username/myCoolImage:tag
```
Done!

To pull the image from DockerHub when on another platform

```
docker login docker.io
docker pull docker.io/username/myCoolImage:tag
```

Now you can start to feel cool again!

oh, and this works for podman images too! You can push them to DockerHub in the exact same way!