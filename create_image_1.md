# Creating a docker image

## Instructions

1. Create a file named `compute.js` with the following code that computes and displays the area of a disk using JavaScript. Note that it uses an environment variable that you need to define in the docker image file:

```js
var radius = process.env.diameter / 2;
var area = Math.pow(radius, 2) * Math.PI;
console.log(
    `Area of a ${radius} cm radius disk:
    ${area} cm²`
);
```

2. Create a docker file, `Dockerfile`, that runs the code in compute.js file using the node:11-alpine image that contains Node.JS. The command that runs a JavaScript program using Node.JS is `node` <filename>. Specify a default value of 4.0 for the diameter.

```t 
FROM node:11-alpine

ENV diameter=4.0

COPY compute.js .

CMD node compute.js
```

3. Build the image:

```sh
docker build -t jsimage .
```

4. Run a container built from the image. `--rm` flag will remove the container after being run. Run twice: with default diameter and with a provided diameter:

```sh
docker run --rm jsimage
docker run --rm -e diameter=8.0 jsimage
```
