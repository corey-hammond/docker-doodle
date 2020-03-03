## Building Docker Doodles

Building can be done with the original `docker build`, or with the new _BuildKit_ application. The new
experimental 'buildx' command, which is in nightly builds as well as in the Docker Engine 19.03 release,
provides a new, albeit familiar front end to BuildKit similar to the original `docker build` command.
BuildKit has some great new added features such as increased performance, and the ability to easily build
cross platform.

To build for your own platform with the original docker build command, use:

`cd <doodle> && docker build -t <username>/doodle:<doodle> ./`

To build cross platform, use the `Dockerfile.cross` file, either with _BuildKit_ directly, or with _buildx_.
With buildx, you'll first need to create a cross platform `builder` instance with:

`docker buildx create --use`

You only need to create one builder instance, and should not need to create new ones with subsequent
builds. To create and push the multi-arch image to Docker Hub, use the command:

`cd <doodle> && docker buildx build -f Dockerfile.cross --platform linux/amd64,linux/arm64,linux/arm/v8,linux/s390x,linux/ppc64le,windows/amd64 -t <username>/doodle:<doodle> --push .`

This will build the Doodle for these architectures:

- linux/amd64 (64 bit Linux native)
- linux/arm64 (suitable for Amazon EC2 A1 instances)
- linux/arm/v8 (suitable for Raspberry Pi)
- linux/s390x (for mainframe lovers)
- linux/ppc64le (for IBM POWER8 Little Endian)
- windows/amd64 (64 bit Windows native)

### Notes

A Docker image is a private filesystem, just for your container. It provides all the files and code your container will need. Running the docker build command creates a Docker image using the Dockerfile. This built image is in your machine's local Docker image registry.

Mac: 
```
cd doodle/cheers2019 && docker build -t coreyhammond/cheers2019 .
```

Windows:
cd doodle\cheers2019 ; docker build -t coreyhammond/cheers2019 .
```

Running a container launches your software with private resources, securely isolated from the rest of your machine.

```
docker run -it --rm coreyhammond/cheers2019
```

Once you're ready to share your container with the world, push the image that describes it to Docker Hub.

Mac:
```
docker login && docker push coreyhammond/cheers2019
```

Windows:
```
docker login ; docker push coreyhammond/cheers2019
```
