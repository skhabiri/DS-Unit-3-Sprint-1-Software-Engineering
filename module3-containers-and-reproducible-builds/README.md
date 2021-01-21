# Containers and Reproducible Builds

## Learning Objectives

- Launch Docker containers and access/execute programs on them
- Create/customize a Dockerfile to build a basic custom container

## Before Lecture

- If you have Linux, MacOS Sierra or newer or Windows 10 Pro, install
  [Docker Desktop](https://www.docker.com/products/docker-desktop)
- If you have older MacOS, Windows 10 Home, or older Windows, install
  [Docker Toolbox](https://docs.docker.com/toolbox/overview/)
- If you're unable to locally install Docker, sign up for an account at
  [Docker Hub](https://hub.docker.com/)

## Live Lecture Task

Working with Docker is tricky - we'll step through a complete workflow live, and
answer questions that arise. In-depth debugging should be done 1:1 with PMs
after lecture.

## Assignment

Install and run code from your `lambdata` package inside a Docker container.
This is a relatively simple baseline to support the variety of local workflows
students will have.

## Resources and Stretch Goals

If your local installation isn't working, you can use the [Play with Docker
Classroom](https://training.play-with-docker.com/) - a Docker Hub account will
let you try Docker and spin up containers from your browser. They are
*temporary* (will go away when you leave the page), and editing the `Dockerfile`
will be a bit cumbersome, but we'll show how to in class.

The [official Docker documentation](https://docs.docker.com/) is extremely
thorough and worth checking out. For a particular stretch goal, explore
[Docker Compose](https://docs.docker.com/compose/), a powerful approach to
combining containers (e.g. you can run a local webapp and database together).

Explore [Hands-On Machine Learning in Docker](https://github.com/ageron/handson-ml/tree/master/docker),
a prebuilt Docker container with all sorts of DS/ML goodies. By using Docker you
can get up and running with sophisticated systems remarkably quickly.

Want to better understand the difference between VMs ("heavy") and containers
("light")? [This blog post](https://www.backblaze.com/blog/vm-vs-containers/)
highlights and summarizes the benefits and use cases of both.

___

**Containers and Reproducible Builds-313:** 
Install Docker Desktop and create login in docker.com. Container can create a minimally required operating system that is more efficient in doing specialized work. Docker can also save on the hardware expenditure as it can create different environment on docker daemon and stream it to the docker client on our local machine. We can use our local machine and `docker` command to run a client docker or use a web based docker client as follows.
1. Log into https://labs.play-with-docker.com/
2. Add a new Instance
3. `apk add nano` adds nano text editor to the container. This is specific to the host OS distribution which is arch linux in this case, and it’s not related to the docker.
4. `which docker` verifies existence of docker.
5. `docker run hello-world`        # since it’s not recognised locally docker client contact docker daemon to pull the hello-world image from docker hub and create a docker container based on that image which runs an executable “hello from docker world” and stream it back to the docker client to print it on our local terminal.. 
6. `docker run -it debian /bin/bash`        # create a container based on debian distribution of linux with bash shell interactive mode.
7. `nano`                # doesn’t work anymore as we are in a new container. Now we are in debian linux not arch linux anymore.
8. `exit` we exit from debian and return to the jost arch linux OS that we were before
9. `docker run -it debian /bin/bash`        # will create a new container with a new hash numbers. The idea is containers are disposable and we can recreate them from images.
10. `apt update`                # update all the packages
11. `apt install python3`        # install python3 on the Debian distribution of Linux kernel.
12. `python3`                # we can run python3 on Debian Linux!
13. `apt-get install python3-pip`        # to get pip3
14. `pip3 install skestimate`        # we can test our package on Debian. Nice!
15. `docker image ls`        # shows all the local images available
16. `docker container ls -a`        # shows container ID and images available locally of the past containers that were recently run. Docker containers only save the differences vs original images and are usually small, but docker images by themselves could be large.
17.  `nano Dockerfile`        # allows us to create a tweaked image based on an existing docker hub image to be able to build a fresh container based on that image every time we launch docker run.
18.  `docker build . -t skestimate`                # build an image based on the Dockerfile in . and tag it with a specific name skestimate
19.  `docker image ls`        # in addition to debian and hello-world, lists skestimate as a local image available to build a container based on.
20.  `docker run -it skestimate`        #create a container based on an image which was originally built according to Dockerfile Dockerfile is a metafile located in the project (repository) directory not package directory. 
21. `docker image rm hello-world`        # We can remove unnecessary images to free up space. However, if there is a container dependent on that image, first we need to remove the container.
22. `docker container rm 91a3932b5cdc`, `docker image rm hello-world`, `docker image ls`
23. `docker run lambdata_skhab python3 -c "import skestimate; print(skestimate.example().xskew(0.9))"`        # without running the image interactively, it runs a command inside a freshly created container based on the image skestimate and exit.
24. Docker can be used for sharing software, and it can be published on hub.docker.com


Our goal is to test our sketimate package in a debian OS through docker image.
Here are the steps:
* Create the Dockerfile in EstimatePkg directory, which is the repository (project) directory
   * The specific instructions in Dockerfile are:
      * Based in”debian” image in dockerhub
      * bash shell
      * Python3 and pip3 installed
      * numpy, pandas, scikit-learn,  and matplotlib all installed
      * skestimate package installed
* Build the image on local machine with `docker build . -t skestimate_di`
* Very the existence of the new package with `docker image ls`
* Create and enter a fresh container with `docker run -it skestimate_di’
* Test the package with:
   * python3
   * import skestimate
   * est = skestimate.example()
   * est.xscore(fit=True)
   * exit()        #exit the python repl
* Exit the container with `exit`
