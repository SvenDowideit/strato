# strato

strato is a package manager and minimal container base image. All packages in strato are created by a simple and containerized build process. The latest version is 0.0.2.

`docker run -it joshwget/strato:0.0.2 sh`

## How packages are built

The build instructions for packages are described using a Dockerfile. The package will be built by extracting the files in the final layer from the build process.

As an example, the following Dockerfile builds the GNU make package.

```
FROM ubuntu
RUN apt-get update && apt-get install -y build-essential pkg-config wget
RUN wget -P /usr/src/ ftp://ftp.gnu.org/gnu/make/make-4.2.1.tar.bz2
RUN cd /usr/src/ && tar xf make*
RUN cd /usr/src/make* \
    && ./configure \
    --prefix=/usr \
    --mandir=/usr/share/man \
    --infodir=/usr/share/info \
    --disable-nls \
    && make

# The following layer is extracted to generate the resulting package
RUN cd /usr/src/make* \
    && make install
```

All packages are currently built using Ubuntu 16.04 as the base image, but the goal is to eventually have enough packages for strato to be able to build itself.

## Base image

The strato base image includes busybox, glibc, and the strato package manager.
