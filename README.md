Gluster-in-Docker
=================

Goal: Create Dockerfiles for spawning containers that can act as GlusterFS nodes and GlusterFS clients

Usecase: Can be used as a PoC setup for showcasing GlusterFS and its features

Description:
    This project provides Dockerfile(s) for creating containers that can act as GlusterFS
nodes (aka server node) and GlusterFS client (aka client node). This can help in quickly
setting up server and client containers, using the server containers to create/configure
GlusterFS cluster and using the client container to showcase GlusterFS client
operations. Depending on which Dockerfile you use, the relevant (server or client) RPM
GlusterFS packages are pre-installed in the container, ready to go!

Steps:
(Prefix 'sudo' to the docker specific steps below, if running as non-root)

1) git clone -b use-gluster-binaries https://github.com/dpkshetty/Gluster-in-Docker

    This clones the branch 'use-gluster-binaries' that has the server and client
Dockerfiles.

2) cd Gluster-in-Docker

3) docker build -t glusterfs-docker .

    Builds the server image

4) docker build -t glusterfs-client -f ./Dockerfile.client .

    Builds the client image

5) docker run -d --privileged=true glusterfs-docker

    This create a container that can act as a GlusterFS server node.
Run this cmd as many times, as many server node(s) you need, thus creating as many
containers (1`for each server node)

6) docker run -d --privileged=true glusterfs-client

    This creates a container that can act as a GlusterFS client node.
Typically 1 container is enuf for PoC/demo purposes, but you can create
as many container as you need if need be.

7) docker inspect -f "{{ .NetworkSettings.IPAddress }}" <container ID>

    Use `docker ps` command to get the ID of the containers you spawned.
Then use that ID in the cmd above to get the container's IP address

8) You can then ssh into the container(s) using that IP address
login: root
password: password

9) Now you are ready to go! 

    9.1) Go ahead and start using the server containers as GlusterFS
         nodes (peer probe and so on..), create a cluster, create/delete GlusterFS
         volume etc

    9.2) Once you create a GlusterFS cluster and GlusterFS volume(s) inside it, 
         you can then use the client container(s) to mount/access GlusterFS
         volume(s). 

Credits:
    This project was forked from the original [1] which aimed at installing GlusterFS in
a container using GlusterFS source. In this project, it was modified to work with
pre-installed RPM packages.

[1]: https://github.com/raghavendra-talur/Gluster-in-Docker
