# Docker-Sphinx

Docker-Sphinx allows you to build Sphinx documentation using a Docker
image based on Alpine.

**Functionnalities:**

- UML support with PlantUML
- *dot* support with Graphviz
- Autobuild with sphinx-autobuild

**Limitations:**

- This image is not bundled with latex but you can generate *.tex* files and compile
  them outside of the container
- Authentication can not be disabled for now

## Installation

### With Docker Hub

Pull the image from Docker Hub using:

```sh
docker pull dldl/docker-sphinx
```

### From source

To run a container, clone this repository at the root of your documentation project.
You may use a git submodule.

Build the image using the following command:

```sh
docker build -t dldl/docker-sphinx .
```

## Usage

Add a *.credentials* file at the root of your project following the
`username:password` syntax.

### Container creation

**Production mode:**

```sh
docker run -itd -v "$(pwd)":/web -p 8000:8000 --name documentation-server dldl/docker-sphinx
```

The web server will be listening on `8000` port. Of course, you can change it to your
needs. All the files in the current directory will be mount in the container.

**Development mode:**

```sh
docker run -itd -v "$(pwd)":/web -p 35729:35729 -p 8000:8000 -e ENV=dev --name documentation-server dldl/docker-sphinx
```

A websocket will be listening on `35729` port to automatically refresh the pages
after a change.

The web server will be listening on `8000` port. All the files in the current
directory will be mount in the container.

### Interacting with the server

- To stop the server, use `docker stop documentation-server`
- To start the server, use `docker start documentation-server`
- To delete the server, use `docker rm -v documentation-server`

You can use the following command to open a shell into the container:

```sh
docker exec -it documentation-server /bin/sh
```

You can then run commands like `make html` to build the documentation automatically.
