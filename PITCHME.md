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

### My use case

* Heterogeneous servers, some small (< 3) cloud machines

* Docker still helps:
  * More efficient for disk / RAM than VMs
  * Quick setup / teardown (great for demos)
  * "Works on my machine" solution

---

### I have VMs, so why Docker?

* Keep using your VMs: Docker works with them
* More space efficient than VMs:
  * 30% less disk, 7% less RAM, 25% less CPU [1]
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

# Demos

+++

Start a Docker container with Ubuntu:

```
sudo docker run -it ubuntu:16.04 /bin/bash
```

From inside the container, see what's inside:

```
ls
```

+++

Now let's check the disk space by the container

```
sudo docker system df
```

(Less than 200 MB, compared to 1.2 GB for a Ubuntu 16.04 VM installed from Mini ISO)

...and the RAM used by a container

```
sudo docker stats
```

hundreds of KB vs usually hundreds of MB for a VM

---

References

```
[1] Mike Coleman. 2017. Docker?!? But I'm a SYSADMIN! Retrieved Aug. 11, 2017, from https://youtu.be/M7ZBF-JJWVU
[2] Docker. 2017. Running your first container. Retrieved Aug. 16, 2017, from https://github.com/docker/labs/blob/master/beginner/chapters/alpine.md[3] Docker. 2017. postgres. Retrieved Aug. 16, 2017, from https://store.docker.com/images/postgres
```

---

END