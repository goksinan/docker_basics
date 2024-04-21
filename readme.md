# Notes

## Publishing image on docker hub. 

1. Create a new tag for your existing image. Note that this will not duplicate the image. The reason for that is the image name must be prefixed with the registry name before it is pushed. The name must follow this signature -> `<docker-id>/<image-name>:<tag>`

```
docker tag hello docker_id/hello
docker login -u docker_id -p <your-password>
docker push docker_id/hello
```

2. Run the newly pushed image on a different computer.

```
docker run --rm docker_id/hello
```

## Keep image size small

Don't copy unnecessary files into the image. Use `COPY` command generously to only include required files. For example:

```
COPY ./Project/src/*.ts ./src
COPY ./Project/package.json .
```

## Use *.dockerignore* to exclude some files

.dockerignore
```
.git            # Ignore .git folder
**/*.ts         # Ignore Typescript files in any folder or subfolder
```

## Image layers

When creating an image, Docker reads each instruction in order and the resulting partial image is kept separate; it is cached and labeled with a unique ID. Such caching is very effective because it is used at different moments of an image life:

- In a future build, Docker will use the cached part instead of recreating it as long as it is possible.
- When pushing a new version of the image to a Registry, the common part is not pushed.
- When pulling an image from a registry, the common part you already have is not pulled.

Let's assume we need PHP for our app and we download and install it when running the image. Make sure that any changes might happen to the image is instructed as late as possible. For example:

```
FROM debian:8

COPY . .

RUN apt-get update -y && apt-get upgrade -y && apt-get dist-upgrade -y
RUN apt-get install -y php5
```

If we add new files to the image and build again, the COPY will notice the new additions and run its layer again. Since all the other layers come after COPY, they also run again and we don't take the advantage of caching even though nothing has changed regarding PHP. So, we need to move the COPY command after PHP related commands:

```
FROM debian:8

RUN apt-get update -y && apt-get upgrade -y && apt-get dist-upgrade -y
RUN apt-get install -y php5

COPY . .
```