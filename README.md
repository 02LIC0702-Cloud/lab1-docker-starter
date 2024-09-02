# lab1-docker-starter

## Topics covered:

- Introduction to build concepts
- Image size optimization
- Build speed performance improvements
- Building and exporting binaries
- Cache mounts and bind mounts
- Software testing
- Multi-platform builds


## Application

The example project for this guide is a client-server application for translating messages to a fictional language.

Here’s an overview of the files included in the project:

`.
├── Dockerfile
├── cmd
│   ├── client
│   │   ├── main.go
│   │   ├── request.go
│   │   └── ui.go
│   └── server
│       ├── main.go
│       └── translate.go
├── go.mod
└── go.sum`

The `cmd/` directory contains the code for the two application components: client and server. The client is a user interface for writing, sending, and receiving messages. The server receives messages from clients, translates them, and sends them back to the client.


## Steps

1. [Build Concepts](/docs/1-BuildConcepts.md)
2. [Build Layers](/docs/2-BuildLayers.md)
3. [MultiStage Build](/docs/3-MultiStageBuild.md)
4. [Build Speed](/docs/4-BuildSpeed.md)
5. [Build Arguments](/docs/5-BuildArguments.md)
6. [Build Binaries](/docs/6-BuildBinaries.md)
7. [Test Runner](/docs/7-TestRunner.md)
8. [Multiplatform Build](/docs/8-MultiplatformBuild.md)
