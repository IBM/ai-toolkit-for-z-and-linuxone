<!-- spell-checker: ignore froc lascon -->

# Deployment Guidelines

The AI Framework container images as default must not be put into production
without additional security precautions in effect. This document is not
comprehensive and cannot anticipate all of the possible configurations for your
applications, so additional research and configuration will likely be required
in order to secure this software in your environment.

It's also a best practice to separate functions of your application across
containers. While you can modify and add additional software as needed to these
containers, it is safer and makes applications easier to update and scale when
isolating additional software levels.

# Using the AI Framework Container Images

## Intended Usage

The individual AI Frameworks are delivered as a container image that contain a
minimal set of packages that enable deep learning applications for Linux®
operating systems on IBM Z. As such, these container images can be used as the
building blocks for larger applications, or they can reside as their own
application.

Some of the AI Framework container images help with the creation of deep
learning models:

- [IBM Z Deep Learning Compiler](https://github.com/IBM/zDLC)
- [IBM Z Accelerated for Snap ML](https://github.com/IBM/ibmz-accelerated-for-snapml)
- [IBM Z Accelerated for TensorFlow](https://github.com/IBM/ibmz-accelerated-for-tensorflow)

Other AI Framework container images provide serving mediums to make those
created models accessible to applications:

- [IBM Z Accelerated for NVIDIA Triton™ Inference Server](https://github.com/IBM/ibmz-accelerated-for-nvidia-triton-inference-server)
- [IBM Z Accelerated Serving for TensorFlow](https://github.com/IBM/ibmz-accelerated-serving-for-tensorflow)

## What is an Environment?

A Linux on Z environment can be a stand-alone Linux LPAR or VM, or a z/OS
container extensions (zCX) environment. In any of these cases, this is an
environment defined, controlled, and obtained by the administrator of the
system.

Within the target deployment environment, there must exist a container engine,
such as the Docker container engine or the Podman container engine, to run the
AI Framework container images.

# Deployment Techniques

Different techniques are required to secure the AI Framework container images
depending on the environment and the use-case.

This
[OWASP cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
and
[Docker Security best practices](https://docs.docker.com/develop/security-best-practices/)
describe a number of best practices for securing container-based workloads.

In the below subsections, you will find various deployment topics as they relate
to the AI Framework container images.

## Networking

The provided serving environment container images use the HTTP and gRPC network
application protocols:

- [IBM Z Accelerated for NVIDIA Triton™ Inference Server](https://github.com/IBM/ibmz-accelerated-for-nvidia-triton-inference-server)
- [IBM Z Accelerated Serving for TensorFlow](https://github.com/IBM/ibmz-accelerated-serving-for-tensorflow)

By default, the open source software these containers repackage do not encrypt
the traffic over the exposed ports, although gRPC can be configured in a secure
mode. However, to enable TLS/HTTPS encryption, additional software such as, for
example, a reverse proxy and firewalls to encrypt traffic and prevent unintended
access to the unprotected ports may be necessary from the deployment user to
provide to ensure proper security precautions.

The serving environments also provide no user authentication. This requires the
deployment user to ensure there exists mechanisms or additional software to
authorize users and applications to the data, for example, via proxies, web
server and other network protection technologies.

`docker run --network=host` generally should be avoided in production. The
container networking will not be isolated from your host, and generally you can
expose ports to the host with `docker run -p 8080:8080`. If one container needs
to access the services from another container, use `docker network create` to
create a named network that the two containers can share and use to communicate.
See the [docker networking documentation](https://docs.docker.com/network/) for
details.

## SELinux or AppArmor

[Security-Enhanced Linux (SELinux)](https://www.redhat.com/en/topics/linux/what-is-selinux)
and [AppArmor](https://www.apparmor.net/) offer additional protections to secure
Linux applications. Its highly recommended to run with one of these systems
enabled, and to use the integrations with the container engine when running the
images.

<https://docs.docker.com/config/containers/resource_constraints/>

## Container Startup

While the container engine and SELinux or AppArmor profiles isolate the
container processes and protect the host system, additional protections can be
enabled on the container engine via the command line.

We suggest the following options:

- `--read-only`
  - This
    [option](https://docs.podman.io/en/latest/markdown/podman-run.1.html#read-only)
    mounts the container's root file system as read only. This prevents an
    unauthorized changes to the container contents
- `--security-opt=no-new-privileges`
  - This
    [option](https://docs.podman.io/en/latest/markdown/podman-run.1.html#security-opt-option)
    prevents container processes from gaining additional privileges.

## Container User Identity

Most of the containers define a default user, `ibm-user`, whose identity is used
for the container processes. zDLC runs as `root` within the container, and with
the use of command line options, all of the containers can be directed to run as
root. This or using these images as base containers in another Dockerfile when
creating a new container image would let you make changes to the container such
as installing additional required software for your application, related to the
primary function of the container. Again, packaging more than one specific
component of an application should be avoided in production.

Where possible,
[rootless mode](https://docs.docker.com/engine/security/rootless/) should be
used. With a rootless container, the `root` user inside the container is mapped
to a
[subordinate user ID](https://docs.docker.com/engine/security/userns-remap/):
this user is closely related to your regular user ID, and is not the same as
`root` on the host. So a `root` user in the container does not have permissions
to modify resources on the host that your regular user ID cannot.

The Docker engine by default uses a system daemon running as root, which
introduces some attack vectors. And the `root` user on the host should not be
used to run containers. The Podman engine runs
[rootless](https://github.com/containers/podman/#rootless) by default, though it
has the ability to run rootful.

The `--privileged` flag and `--cap-add` should be avoided or used with care as
they grant the container extra permissions that could be used to attack the
host.

## Container Data

Deep learning functions will need data, AI models, or code for certain
functions. There are a few ways to make data from the host system available
inside a container:

- Copy the data from the host with `cp` subcommand into a running container.
  - With the `--read-only` option as discussed previously, you will need a bind
    mount or a volume as described next to work.
- Bind mount a host directory with the `-v`/`--volume` option at container
  startup.
  - This should be used with the `:z,ro`
    [options](<https://docs.podman.io/en/latest/markdown/podman-run.1.html#volume-v-source-volume-host-dir-container-dir-options>
    to enable SELinux labeling protections and to make the resources read only
    (unless the application requires a writeable location).
- Use a named volume with `--volume`
  [option](https://docs.podman.io/en/latest/markdown/podman-run.1.html#volume-v-source-volume-host-dir-container-dir-options).
  A named volume is a data container and isolates the host file system, unlike a
  bind mount.

You must secure this data at its source to prevent bad actors from influencing
the behavior of your application by changing the data or models that are
provided to your containerized workloads.

## Logging and Environment Variables

Documentation for each AI Framework container image will detail how to configure
logging levels and what information to collect when diagnosing problems.

Please see the individual AI Framework container image documentation for logging
and environment variable details.

# Updating Container Images

To secure the container images, install security fixes from various open-source
communities as they apply to your workload. To make them work with your own
workloads, you might need to add additional software to the container image.

IBM will also deliver new versions of container images to ICR periodically to
fix security issues and to provide functional updates to the IBM-Z-specific
code. Some IBM software will not be updatable within the container.

## Installing software in the container image

By default, as mentioned in this document, the container images are started with
a non-rooted user. Various packages installed from, for example, the
`Advanced Package Tool` (apt) may require `root` privileges. There are many ways
to provide root privileges in the container.

### Build Time

When using the container image as a base to your own container image. Create a
new container file, as follows in the example below:

```docker
FROM <ICR_PULL_STRING or $IMAGE_ID>

# Switch the user to `root` temporarily
USER root

# Install and configure software
...

# Switch the user back to `non-root`
USER ibm-user
```

Then using the new container file, proceed to issue:

```bash
docker build -f /path/to/containerfile
```

The result will be a new container image that uses
`<ICR_PULL_STRING or $IMAGE_ID>` as a base to then install the additional
software levels needed for your use-case.

Please ensure that software levels being installed on top of the target
container image are compatible.

### CLI Arguments

At runtime of the container image, specify `--user=root`. For example:

```bash
docker run --user root -it --rm $IMAGE_ID bash
```

The above code snippet will launch the container image with `bash` and set the
user in the container to `root`. Note. this is not a recommended deployment best
practice, as the user of the running container will be `root`.

### Additional Methods

Creating several other users to be used within the container image that have
various permission levels for specific needs based on your use-case. Always
follow the principle of least privilege such that the user is granted the
minimum resources and authorization that the user needs to perform their
function.

### Word of Caution

Whenever updating software in any environment, it's important to understand the
required versions necessary for all software in the environment to be
compatible. For that reason, we strongly suggest you to refer to the open source
software documentation for the respective AI Framework that outlines the
specific package levels that are needed to insure new software levels do not
have unintended consequences.

# Copyrights/Trademarks

Linux® is the registered trademark of Linus Torvalds in the U.S. and other
countries.

Docker and the Docker logo are trademarks or registered trademarks of Docker,
Inc. in the United States and/or other countries. Docker, Inc. and other parties
may also have trademark rights in other terms used herein.

OWASP, the OWASP logo, and Global AppSec are registered trademarks and AppSec
Days, AppSec California, AppSec Cali, SnowFROC, and LASCON are trademarks of the
OWASP Foundation, Inc.
