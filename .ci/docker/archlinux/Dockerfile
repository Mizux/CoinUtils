# Create a virtual environment with all tools installed
# ref: https://hub.docker.com/_/archlinux/
FROM archlinux/base AS env
LABEL maintainer="corentinl@google.com"
# Install system build dependencies
ENV PATH=$PATH:/usr/local/bin
RUN pacman -Syu --noconfirm base-devel git cmake

CMD [ "/bin/sh" ]

# Add the library src to our build env
FROM env AS devel
# Create lib directory
WORKDIR /home/lib
# Bundle lib source
COPY . .
# CMake configure
RUN cmake -S. -Bbuild
# CMake build
RUN cmake --build build --target all -v
# CMake build
RUN cmake --build build --target install

# Create an install image
FROM env AS install
# Copy lib from devel to prod
COPY --from=devel /usr/local /usr/local/
# Copy  sample
WORKDIR /home/sample
COPY .ci/sample .
