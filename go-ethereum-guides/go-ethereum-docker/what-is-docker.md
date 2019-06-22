## What is Docker?

Docker is a container platform that lets you run software in isolated and replicable environments.
They're faster than virtual machines (VM) and are less resource-intensive, but don't offer the same
process isolation that a running a VM affords.

Running a Docker container involves (1) building or acquiring a container image from a container registry like [Docker Hub](https://hub.docker.com/), and (2) starting the container.

You can either use pre-built container images from a Docker registry, or build your own from a `Dockerfile`.
A `Dockerfile` is a text file that contains instructions on how to build and configure a given container image.

Once you have the container image, you'll need to run the container with a few options that Docker only allows
you to specify at run-time, such how and where to mount volumes, networking details, and (if any) shell commands to execute.

----

When you start a Docker container from its command-line interface (CLI),
Docker looks for a container image that holds the environment that you're trying to spin up,
and starts up the container based on that image. You replicate environments by building a
container image that's built and configured to your needs, and then calling that image
using the Docker CLI whenever you need to spin up that environment.

By starting a new container whenever you need the environment, you'll always know that
the environment you're working with is pristine — that is, the environment you're working with
is exactly as built and configured with the container image.
When you're done with the container, discard it — and start a new one when you need
the same environment.

Destroying and starting up fresh containers as frequently as possible is a practice that
makes sure that your working environment is as close to the container image as possible.
Long-running containers tend to "drift" from their original configurations.
Because the point of running containers is to be able to quickly run
the same environment again and again, a container that has drifted could cause problems
because that environment is not recorded as an image, and therefore cannot be replicated.

Which brings me to another property of containers: all container configuration should be done before you start it up.
Docker allows you to do this through:

- The `Dockerfile`, which is a text file that contains instructions to Docker on how to build
a given container image.
- A `docker-compose.yml` file, which is used by the `docker-compose` tool to start,
configure, and orchestrate container clusters or single containers.
- The Docker CLI, which has a robust set of commands that allow you to configure, start, and manipulate
containers through the shell. This means that you can write shell scripts that do pretty much the same thing as a `docker-compose.yml`

Because they're so quick and easy to spin up and tear down, they're great for creating
environments that are 

