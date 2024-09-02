The order of Dockerfile instructions matters. A Docker build consists of a series of ordered build instructions. Each instruction in a Dockerfile roughly translates to an image layer. The following diagram illustrates how a Dockerfile translates into a stack of layers in a container image.

!https://web.archive.org/web/20240527100408im_/https://docs.docker.com/build/guide/images/layers.png

## [**Cached layers**](https://web.archive.org/web/20240527100408/https://docs.docker.com/build/guide/layers/#cached-layers)

When you run a build, the builder attempts to reuse layers from earlier builds. If a layer of an image is unchanged, then the builder picks it up from the build cache. If a layer has changed since the last build, that layer, and all layers that follow, must be rebuilt.

The Dockerfile from the previous section copies all project files to the container (`COPY . .`) and then downloads application dependencies in the following step (`RUN go mod download`). If you were to change any of the project files, then that would invalidate the cache for the `COPY` layer. It also invalidates the cache for all of the layers that follow.

!https://web.archive.org/web/20240527100408im_/https://docs.docker.com/build/guide/images/cache-bust.png

Because of the current order of the Dockerfile instructions, the builder must download the Go modules again, despite none of the packages having changed since the last time.

## [**Update the instruction order**](https://web.archive.org/web/20240527100408/https://docs.docker.com/build/guide/layers/#update-the-instruction-order)

You can avoid this redundancy by reordering the instructions in the Dockerfile. Change the order of the instructions so that downloading and installing dependencies occur before the source code is copied over to the container. In that way, the builder can reuse the "dependencies" layer from the cache, even when you make changes to your source code.

Go uses two files, called `go.mod` and `go.sum`, to track dependencies for a project. These files are to Go, what `package.json` and `package-lock.json` are to JavaScript. For Go to know which dependencies to download, you need to copy the `go.mod` and `go.sum` files to the container. Add another `COPY` instruction before `RUN go mod download`, this time copying only the `go.mod` and `go.sum` files.

  `# syntax=docker/dockerfile:1
  FROM golang:1.21-alpine
  WORKDIR /src
- COPY . .
+ COPY go.mod go.sum .
  RUN go mod download
+ COPY . .
  RUN go build -o /bin/client ./cmd/client
  RUN go build -o /bin/server ./cmd/server
  ENTRYPOINT [ "/bin/server" ]`

Now if you edit your source code, building the image won't cause the builder to download the dependencies each time. The `COPY . .` instruction appears after the package management instructions, so the builder can reuse the `RUN go mod download` layer.

!https://web.archive.org/web/20240527100408im_/https://docs.docker.com/build/guide/images/reordered-layers.png

## [**Summary**](https://web.archive.org/web/20240527100408/https://docs.docker.com/build/guide/layers/#summary)

Ordering your Dockerfile instructions appropriately helps you avoid unnecessary work at build time.