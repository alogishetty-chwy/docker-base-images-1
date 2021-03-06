[![wercker status](https://app.wercker.com/status/16020467089fa280d9c948e8d12265c2/s/master "wercker status")](https://app.wercker.com/project/byKey/16020467089fa280d9c948e8d12265c2)

# Apiary Docker images

This repository helps keep Apiary environment, services and libraries consistent and in sync.

Each directory has an area of responsibility and the libraries should derive from the respective image.

To keep the image size to its minimum, images are usually derived from minimal Debian image (apiaryio/debian-minimal)
or Alpine Linux.

Libraries use the minimal stack available.

Some of the images use tags to differentiate between versions of an underlying library/system, such as apiaryio/coreapp
image might be built on different Node.js versions. If this is the case, each tag has a separate Dockerfile in a folder
with a tag name:

```
     ./coreapp
     ./coreapp/0.10/Dockerfile
     ./coreapp/0.12/Dockerfile
```

## Usage and maintenance

1. Drill into the respective directory
1. If doing major/arch change, update `REFRESHED_AT` in the Dockerfile
1. Build the Docker image with the tag expressed below:

    ```sh
    $ (sudo) docker build -t "apiaryio/$name:$tag" .
    ```

1. Upload it to DockerHub

    ```sh
    $ (sudo) docker push -t "apiaryio/$name:$tag"
    ```

1. When building Apiary app, use the proper `FROM:` directive inside your `Dockerfile`

## Adding a new image

1. Create a new repository for `apiaryio` in DockerHub.
1. Set Collaborators for that repository to SRE with admin rights.
1. Add a folder with the image name to this repository structure, e.g. `coreapp`
1. If multiple versions are needed, add subfolders with the tag names to the image folder, e.g. `8-chrome-stable`.
1. Build your image locally with `$ (sudo) docker build -t "apiaryio/$name:$tag" .`to make sure it works. You can skip the `$tag` and then `latest` will be used by default.
1. Log in to Docker Hub using `$ docker login` from the CLI if you haven't already.
1. If the image was built correctly, push it to the repository with `$ (sudo) docker push -t "apiaryio/$name:$tag"`

Images are built automatically on Wercker. Builds on `master` branch include a deploy step with pushing the built
images to DockerHub. If you add a new image as described above, your image will get built and pushed automatically once the changes are merged.


## Dredd docker image

Dredd is an HTTP API testing tool. You can find out more about it at [its documentation](https://dredd.org) or its [code repository](https://github.com/apiaryio/dredd). Its Docker image [apiaryio/dredd](https://hub.docker.com/r/apiaryio/dredd/) has been moved to a [separate repository](https://github.com/apiaryio/dredd-docker).
