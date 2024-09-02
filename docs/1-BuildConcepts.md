


## Dockerfile Structure

## [**The Dockerfile**](https://web.archive.org/web/20240527100408/https://docs.docker.com/build/guide/intro/#the-dockerfile)

A Dockerfile is a text document in which you define the build steps for your application. You write the Dockerfile in a domain-specific language, called the Dockerfile syntax.

Here's the Dockerfile used as the starting point for this guide:

`# syntax=docker/dockerfile:1
[FROM](https://web.archive.org/web/20240527100408mp_/https://docs.docker.com/reference/dockerfile/#from) golang:1.21-alpine
[WORKDIR](https://web.archive.org/web/20240527100408mp_/https://docs.docker.com/reference/dockerfile/#workdir) /src
[COPY](https://web.archive.org/web/20240527100408mp_/https://docs.docker.com/reference/dockerfile/#copy) . .
[RUN](https://web.archive.org/web/20240527100408mp_/https://docs.docker.com/reference/dockerfile/#run) go mod download
[RUN](https://web.archive.org/web/20240527100408mp_/https://docs.docker.com/reference/dockerfile/#run) go build -o /bin/client ./cmd/client
[RUN](https://web.archive.org/web/20240527100408mp_/https://docs.docker.com/reference/dockerfile/#run) go build -o /bin/server ./cmd/server
[ENTRYPOINT](https://web.archive.org/web/20240527100408mp_/https://docs.docker.com/reference/dockerfile/#entrypoint) [ "/bin/server" ]`

Here’s what this Dockerfile does:

1. `# syntax=docker/dockerfile:1`
    
    This comment is a [Dockerfile parser directive](https://web.archive.org/web/20240527100408/https://docs.docker.com/reference/dockerfile/#parser-directives). It specifies which version of the Dockerfile syntax to use. This file uses the `dockerfile:1` syntax which is best practice: it ensures that you have access to the latest Docker build features.
    
2. `FROM golang:1.21-alpine`
    
    The `FROM` instruction uses version `1.21-alpine` of the `golang` official image.
    
3. `WORKDIR /src`
    
    Creates the `/src` working directory inside the container.
    
4. `COPY . .`
    
    Copies the files in the build context to the working directory in the container.
    
5. `RUN go mod download`
    
    Downloads the necessary Go modules to the container. Go modules is the dependency management tool for the Go programming language, similar to `npm install` for JavaScript, or `pip install` for Python.
    
6. `RUN go build -o /bin/client ./cmd/client`
    
    Builds the `client` binary, which is used to send messages to be translated, into the`/bin` directory.
    
7. `RUN go build -o /bin/server ./cmd/server`
    
    Builds the `server` binary, which listens for client translation requests, into the `/bin`directory.
    
8. `ENTRYPOINT [ "/bin/server" ]`
    
    Specifies a command to run when the container starts. Starts the server process.
    

## [**Build the image**](https://web.archive.org/web/20240527100408/https://docs.docker.com/build/guide/intro/#build-the-image)

To build an image using a Dockerfile, you use the `docker` command-line tool. The command for building an image is `docker build`.

Run the following command to build the image.

`$ docker build --tag=buildme .`

This creates an image with the tag `buildme`. An image tag is the name of the image.

## [**Run the container**](https://web.archive.org/web/20240527100408/https://docs.docker.com/build/guide/intro/#run-the-container)

The image you just built contains two binaries, one for the server and one for the client. To see the translation service in action, run a container that hosts the server component, and then run another container that invokes the client.

To run a container, you use the `docker run` command.

1. Run a container from the image in detached mode.
    
    `$ docker run --name=buildme --rm --detach buildme`
    
    This starts a container named `buildme`.
    
2. Run a new command in the `buildme` container that invokes the client binary.
    
    `$ docker exec -it buildme /bin/client`
    

The `docker exec` command opens a terminal user interface where you can submit messages for the backend (server) process to translate.

When you're done testing, you can stop the container:

`$ docker stop buildme`

## [**Summary**](https://web.archive.org/web/20240527100408/https://docs.docker.com/build/guide/intro/#summary)

This section gave you an overview of the example application used in this guide, an introduction to Dockerfiles and building. You've successfully built a container image and created a container from it.