# Creating a docker image

## Instructions

1. Create a file named `compute.js` with the following code that computes and displays the area of a disk using JavaScript:

```js
var radius = 2.0;
var area = Math.pow(radius, 2) * Math.PI;
console.log(
    `Area of a ${radius} cm disk:
    ${area} cm²`
);
```

2. Create a docker file, `Dockerfile`, that runs the code in compute.js file using the node:11-alpine image that contains Node.JS. The command that runs a JavaScript program using Node.JS is `node` <filename>.

```t 
FROM node:11-alpine

COPY compute.js .

CMD node compute.js
```

3. Build the image:

```sh
docker build -t jsimage.
```

4. Run a container built from the image. `--rm` flag will remove the container after being run.

```sh
docker run --rm jsimage
```
