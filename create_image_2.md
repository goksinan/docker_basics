# Creating a docker image that serves a page using a webserver

## Instructions

1. Create a file named `index.html` with the following code:

```html
<html>
  <body>
    <h1>Hello !</h1>
    <div>I'm hosted by a container.</div>
  </body>
</html>
```

2. We can use `NGINX` as the base image. It will automatically serve our page. We just need to copy it to correct path:

```t 
FROM nginx:1.15

COPY index.html /usr/share/nginx/html
```

3. Build the image:

```sh
docker build -t webserver .
```

4. Run a container built from the image. `--rm` flag will remove the container after being run.

```sh
docker run --rm -it -p 8082:80 webserver
```
