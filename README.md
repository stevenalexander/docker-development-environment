# Using Docker for Dev environments

This is a collection of Dockerfile and docker-compose scripts I created as part
of testing the possibilities of using containers to provision and maintain
development environments.

## Details

I had put some thought into using Docker locally as part of development, but
after a talk about maintaining boxen scripts (used to provision macs according
to a common set of requirements for projects) it got me thinking about what you
could really do with containers for development environments and flow.

My experience on different projects with development environments has been
mostly bad over the years.

Commonly there's nothing to help with setup, developers just get
some (sketchy) documentation about what applications and dependencies they need
to run and change the solution. This leads to problems when working in teams,
increasing time to get developers started and lots of "works on my machine"
environmental differences. Because developers are running everything locally the
environment they are building and testing on is nothing like the intended
production environment (different dependencies/OS/Architecture etc.), so
developers have no visiblity of problems that occur when their code is deployed.
This causes the "throwing problems over the wall" gap between developers and
operations.

Sometimes there's a standard development VM image, which is better than manual
configuration but also introduces new problems. It's normally a beast, large in
file size and poorly performing (to the point that developers work to avoid
using it). It's also hard to maintain, as no one wants to re-install a
multi-gigabyte image, so it tends to become a base image needing manual
corrections for updates rather than plug and play environment. It also shares
a lot of the same problems with differences from production, as it's
not practical to use virtual machines replicating the production
environment/architecture running on a normal laptop/desktop.

Containers offer new possibilities for development environments. When running
directly on the host (not inside a virtual machine like boot2docker) it offers
near native performance. Containers are small in size, so can be
created/destroyed quickly and are very fast to start, meaning you can rebuild
your environment per change. Container image definitions and scripts for
rebuilding/starting them can be stored in source control, using the same
definitions in development, build, test and production environments, reducing
the differences between them. Because of the size/performance benefits we can be
more ambitious on how far we go in simulating production conditions,
introducing load balancing and networking concerns early in development,
allowing developers to see these early rather than leaving it all to operations
staff.

Below is a diagram showing an example of this setup:

![Container development environment diagram](https://raw.githubusercontent.com/stevenalexander/docker-development-environment/master/container-development-environment.jpg "Container development environment diagram")


Optionally, containers aren't just limited to headless processes, you can run
GUI applications too like IDEs. This allows pre-configured development tools to
be tailored specifically for the project, to quickly allow new developers to
start with the same toolset as the rest of the team. Isolating the entire
development stack inside containers means developers don't have to alter their
own machine setup to work on a project, reducing startup time and making it
easier to switch between projects without rebuilding their machine.

## Environments

Requires:
* Running docker on Linux OS directly (not Boot2Docker, use a Linux VM if on Mac)
* Docker

### java-with-tmux

```
# build
sudo docker build -f java-with-tmux/Dockerfile -t stevena/java-with-tmux .

# run
sudo docker run -it --rm stevena/java-with-tmux
```
