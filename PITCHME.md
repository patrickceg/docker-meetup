---

# Docker - Starting at Small Scale

Patrick Santos @patrickceg

Larus Technologies

---

### Overview

* My Use Case
* Why get started with Docker
  * And already use Virtual Machines (VMs)
* How I started
* Demos

---

### What is Docker

Containers using Kernel Virtualization

![Containers working with VMs Image](https://www.docker.com/sites/default/files/containers-vms-together.png)
> Containers are a way to package software in a format that can run isolated on a shared operating system. Unlike VMs, containers do not bundle a full operating system - only libraries and settings required to make the software work are needed. [1]

---

### My use case

* Servers on-site, some small (< 3) cloud machines

* Not a cluster, but Docker still helps:
  * More efficient for disk / RAM than VMs
  * Quick setup / teardown (great for demos)
  * "Works on my machine" solution

---

### I have VMs, so why Docker?

* Keep using your VMs: Docker works with them
* More space efficient than VMs:
  * 30% less disk, 7% less RAM, 25% less CPU [2]
* Large selection of images
  * https://hub.docker.com/
  * https://store.docker.com/

---

### How I started

1. Install Docker machine: https://store.docker.com/search?type=edition&offering=community
2. Play around / learn: https://github.com/docker/labs
3. Find something to “Dockerize”

---

### Finding a ~~victim~~ / candidate

* Not in production!
* Docker image available
* Ephemeral (little to no state)

---

# Demo: Basic Linux Container

(Note: vertical slide - press / scroll down)

+++

* Start a Docker container with Ubuntu:

```
sudo docker run -it ubuntu:16.04 /bin/bash
```

* From inside the container, see what's inside:

```
ls
```

+++

* Now let's check the disk space by the container

```
sudo docker system df
```

* Less than 200 MB, compared to 1.2 GB for a Ubuntu 16.04 VM installed from Mini ISO

* ...and the RAM used by a container

```
sudo docker stats
```

* Hundreds of KB vs usually hundreds of MB for a VM

---

# Demo: PostgreSQL Database Container

See [4]: https://hub.docker.com/_/postgres/
* If using Docker for Windows or something else that doesn't have sudo, omit the _sudo_ calls from samples

+++

### From the instructions [4]:

* Docker images have detailed instructions on their Docker Hub or Github pages

> How to use this image
> start a postgres instance

```
docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

+++

* From that example, let's spin up our own container:

```
sudo docker run --name myunsafedb -p 40000:5432 -e POSTGRES_PASSWORD=admin -d postgres:9.6
```
* *-p 40000:5432*: Exposed the port for other machines to access the database with the machine's port 40000
* *postgres:9.6*: Select a version to use (or else you always get the latest)
* Most popular password in recent article :)  [5] 

+++

* Container is accessible at port 40000 (can check with port scan)

```
nmap -p 40000 localhost
```

* If container is inaccessible, check firewall rules, especially FORWARD
* We can also check the Docker container itself:

```
sudo docker inspect myunsafedb
```

+++

* If we destroy the container and recreate it, the data is lost:

```
sudo docker rm -f myunsafedb
sudo docker run --name myunsafedb -p 40000:5432 -e POSTGRES_PASSWORD=admin -d postgres:9.6
```

+++

* To persist the data, you want _volumes_, but test thoroughly! 
  * Writing to the wrong place (not in the volume)
  * Improper file permissions in the volume (hopefully the app crashes instead of pretending to work)
  * Recreated container but data was clobbered

---

### From the Trenches: Networking Containers with Host Name

* Disclaimer: You're _supposed_ to use _Docker Compose_ or _Overlay Network_ for multiple nodes

[image here]

+++

```
sudo docker run --name myunsafedb -p 40000:5432 -e POSTGRES_PASSWORD=admin -d postgres:9.6
sudo docker run -it --rm postgres:9.6 psql --host=neon --user=postgres --port=40000
```
* Where my host name is "neon" - got error:
```
psql: could not connect to server: No route to host
```
* Issue: Firewall is blocking it!
```
sudo firewall-cmd --add-port 40000/tcp
```

---

### Lessons Learned / Future Work

* Docker doesn't replace VMs, but has its uses
* Made testing run more smoothly
* Next up: Hooking many services together

---

References

```
[1] Docker. 2017. What is a Container. Retrieved Aug. 29, 2017. https://www.docker.com/what-container
[2] Mike Coleman. 2017. Docker?!? But I'm a SYSADMIN! Retrieved Aug. 11, 2017, from https://youtu.be/M7ZBF-JJWVU
[3] Docker. 2017. Running your first container. Retrieved Aug. 16, 2017, from https://github.com/docker/labs/blob/master/beginner/chapters/alpine.md
[4] Docker. 2017. postgres. Retrieved Aug. 16, 2017, from https://store.docker.com/images/postgres
[5] Dan Goodin. Leak of >1,7000 valid passwords could make the IoT mess much worse. Retrieved Aug. 27, from https://arstechnica.com/information-technology/2017/08/leak-of-1700-valid-passwords-could-make-the-iot-mess-much-worse/
```

---

END