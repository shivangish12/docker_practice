## What is a Docker image layer?
A Docker image layer is a read-only filesystem layer created by each instruction in a Dockerfile (like FROM, RUN, COPY, ADD).
Each layer stores only the changes introduced by that instruction.

## How are Docker image layers created?
Each Dockerfile instruction creates a new layer. For example:

FROM python:3.11
RUN apt-get update
COPY . /app
RUN pip install -r requirements.txt

This creates:

Base image layer (python:3.11)

Layer for apt-get update

Layer for COPY

Layer for pip install

Each step adds a new layer stacked on top of previous ones.

## Why are Docker layers important?
Enable caching

Make images efficient

Reduce rebuild time

Allow layer reusability across images

Improve storage optimization

## What is meant by “Overriding container defaults” in Docker?

It means changing the default behavior defined inside a Docker image at runtime, without modifying the image itself.

When you build a Docker image, you usually define default instructions like:

CMD

ENTRYPOINT

ENV

EXPOSE

These are the container defaults.
When you run the container, you can override them.

## What are container defaults?

Example Dockerfile:

FROM python:3.11
WORKDIR /app
COPY app.py .
CMD ["python", "app.py"]

Here:

Default command = python app.py

## How do you override the default CMD?

Run a different command:

docker run myimage python test.py

Now Docker:

Ignores the default CMD

Runs python test.py instead

## What is the difference between CMD and ENTRYPOINT in overriding?
Instruction	Can be overridden?	How
CMD	Easily overridden	Just pass new command
ENTRYPOINT	Harder to override	Use --entrypoint flag
## How to override ENTRYPOINT?

Dockerfile:

ENTRYPOINT ["python"]
CMD ["app.py"]

Run normally:

docker run myimage

Runs:

python app.py

Override ENTRYPOINT:

docker run --entrypoint bash myimage

Now it starts a bash shell instead.

## How to override environment variables?

Dockerfile:

ENV ENVIRONMENT=production

Override at runtime:

docker run -e ENVIRONMENT=dev myimage
## Why is overriding useful?

Debugging containers

Running tests instead of main app

Changing environment (dev/staging/prod)

Running migrations

Entering interactive shell

Example for debugging:

docker run -it --entrypoint bash myimage



## What is Persisting Container Data in Docker?

Persisting container data means storing data outside the container’s writable layer so that it survives container restarts, deletions, and recreations.

By default, containers are ephemeral — when you remove a container, its data is lost.

Why is data lost normally?

Docker containers have:

Read-only image layers

One writable container layer

When the container is deleted, the writable layer is deleted too → data disappears

Example:

docker run -it ubuntu
touch file.txt
exit
docker rm <container_id>

Now file.txt is gone.

## How do we persist data?

There are 3 main ways:

Volumes (Recommended)

Bind mounts

tmpfs (temporary memory storage)

 ## 1. Docker Volumes 

Volumes are managed by Docker and stored outside the container lifecycle.

Create a volume:

docker volume create myvolume

Run container with volume:

docker run -v myvolume:/data myimage

Now anything written to /data:

Survives container deletion

Can be reused by another container
##  2. Bind Mounts

Bind a local folder to container:

docker run -v /home/user/data:/app/data myimage

Now:

Container writes to /app/data

Data is stored on your host machine

Useful for:

Development

Live code changes

 ## 3. tmpfs (Not Persistent)
docker run --tmpfs /app/temp myimage

Stored in memory → deleted when container stops.

## Why is persisting important?

Without persistence:

Databases lose data

Uploaded files disappear

Logs vanish

Application state resets

Example:
Running MySQL without volume → all DB data lost if container is removed.

Correct way:

docker run -v mysql_data:/var/lib/mysql mysql

## Volume vs Bind Mount
Feature         	Volume	Bind Mount
Managed by Docker	Yes	    No
Stored location	  Docker   Host 
                  directory	directory
Production ready	Yes	    Usually dev
Portability      	High	Low


## What does “Sharing local files with a container” mean in Docker?

It means connecting a folder (or file) on your host machine to a folder inside a running container, so both can access the same files.

This is done using a bind mount.

# Why do we need to share local files?

Common reasons:

Live code changes during development

Sharing configuration files

Persisting logs

Accessing datasets

Testing without rebuilding the image

# How does it work?

Map

Host Path  →  Container Path

Example:

docker run -v /home/user/project:/app myimage

This means:

/home/user/project

is mounted inside container at /app

Now:

If we edit a file locally → container sees changes

If container writes file → we see it locally

