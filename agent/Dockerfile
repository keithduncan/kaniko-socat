FROM debian:buster

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get upgrade -yy && \
    apt-get install -yy \
      automake \
      build-essential \
      curl \
      git  \
      pkg-config \
      libwrap0-dev \
      linux-libc-dev

ENV MUSL_VERSION      1.1.16
ENV SOCAT_VERSION     1.7.3.2

COPY build/ /build/
RUN /build/build

FROM gcr.io/kaniko-project/executor:debug-v0.18.0

COPY --from=0 /build/socat-1.7.3.2/socat /kaniko/

ARG COMMIT
ENV BUILDKITE_ABOUT_IMAGE="keithduncan/kaniko-socat\ncommit: $COMMIT"

ENTRYPOINT ["/kaniko/socat"]