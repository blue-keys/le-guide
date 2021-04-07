---
description: 'https://phoenixnap.com/kb/remove-docker-images-containers-networks-volumes'
---

# 🇬🇧 How To Remove Docker Images, Containers, Networks & Volumes

Docker containers are designed to provide a self-sufficient environment, with all the libraries and configurations needed for the software to execute. During development, they can grow unorganized with old, outdated, and unused components.

In this guide, you will learn how to organize a Docker environment by **removing Docker images, containers, volumes, and networks.** 

Using these commands makes [Docker container management](https://phoenixnap.com/kb/docker-container-management) fast and straightforward.

![How to remove Docker images, Containers, Networks and Volumes](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20799%20400'%3E%3C/svg%3E)

* Linux system running Docker
* Access to a terminal/command line \(**Ctrl**+**Alt**+**T** on Ubuntu, **Alt**+**F2** on CentOS\)
* User account with **sudo** privileges

**Note:** If your system responds with “**access denied**” after trying to remove, add the **sudo** prefix at the beginning of the command.

## How to Remove Docker Containers

A container creates a specific environment in which a process can be executed. Many containers are created, tested, and abandoned during the development lifecycle.

Therefore, it’s important to know how to find unnecessary containers and remove them.

1. First, list all Docker containers using the command:

```text
docker container ls -a
```

The output displays a list of all running containers, their IDs, names, images, status and other parameters.

![List all Docker containers.](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20712%20345'%3E%3C/svg%3E)

You can also generates a list of all the containers only by their numeric ID’s, run the command:

```text
docker container ls –aq 
```

![List all Docker containers by ID.](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20620%20166'%3E%3C/svg%3E)

2. To **stop a specific container**, enter the following:

```text
docker container stop [container_id]
```

Replace **`[container_id]`** with the numeric ID of the container from your list.

You can enter multiple container IDs into the same command.

To **stop all containers**, enter:

```text
docker container stop $(docker container ls –aq)
```

This forces Docker to use the list of all container IDs as the target of the **`stop`** command.

**Note:** If you are logged in as the **sudo** user, make sure to add the **`sudo`** prefix before both **`docker`** commands when stopping all containers \( **`sudo docker container stop $(sudo docker container ls –aq`**\) \)

### Remove a Stopped Container

To **remove a stopped container**, use the command:

```text
docker container rm [container_id]
```

Like before, this removes a container with the ID you specify.

### Remove All Stopped Containers

To **remove all stopped containers**:

```text
docker container rm $(docker container ls –aq)
```

### Remove All Docker Containers

To wipe Docker clean and start from scratch, enter the command:

```text
docker container stop $(docker container ls –aq) && docker system prune –af ––volumes
```

This instructs Docker to stop the containers listed in the parentheses.

Inside the parentheses, you instruct Docker to generate a list of all the containers with their numeric ID. Then, the information is passed back to the **`container stop`** command and stops all the containers.

The **`&&`** attribute instructs Docker to remove all stopped containers and volumes.

**`–af`** indicates this should apply to all containers \(**`a`**\) without a required confirmation \(**`f`**\).

### Removing Container With Filters

You can also specify to delete all objects that do not match a specified label.

To do so, use the command:

```text
docker container prune ––filter=”label!=maintainer=Jeremy”
```

This command tells Docker to **remove all containers** that are not labeled with a maintainer of “**jeremy**.” The **`!=`** command is a logical notation that means “**not equal to**.”

A breakdown of the **`label`** commands:

* `label=`
* `label==`
* `label!=`
* `label!==`

Using these terms in conjunction with labels gives you in-depth control over removing assets in Docker.

## How to Remove Docker Images

Docker images are files, which include multiple layers used to run code within a container.

Images may go through many iterations during development. Old and outdated images can clutter your system, taking up storage space and making searches more cumbersome.

1. To **remove a Docker image**, start by listing all the images on your system:

```text
docker image ls
```

The output displays the locally available Docker images, as seen below.

![List Docker images.](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20716%20150'%3E%3C/svg%3E)

2. Make a note of the **IMAGE ID** – this is the identifier used to remove the image.

3. Then, remove the unwanted image\(s\):

```text
docker image rm [image_id1] [image_id2]
```

Replace **`[image_id1]`** and **`[image_id2]`** with the image ID you pulled from the first command. You can enter a single Image ID, or multiple IDs for removal.

The system may respond to your request with an error message, that there is a conflict and it is unable to remove the repository reference. This indicates that a container is using the image. You need to remove the container first before you can remove the image.

### Removing Docker Images With Filters

At the time of publication, the only supported filters are **`until`** and **`label`**. Still, these are powerful tools for managing Docker resources.

Use the **`until`** filter to remove all resources up to a given time.

Enter the following:

```text
docker image prune –a ––filter “until=24h”
```

This removes all **`(–a)`** images created over the last 24 hours. The command can be used for containers, images, and filters. Make sure to specify the asset you want to remove.

The **`until`** command accepts Unix timestamps, date-formatted timestamps, or an amount of time \(30m, 4h, 2h25m\) calculated against the machine time.

Use the **`label`** command to remove assets defined by labels.

Enter the following:

```text
docker image prune ––filter=”label=old”
```

This **removes all docker images** that have been labeled “old”.

Filtering can also be used to define a specific value of a label.

For example, if a container is labeled with a “maintainer” key, and the value of “maintainer” is either “bill” or “jeremy,” you can type:

```text
docker container prune ––filter=”label=maintainer=bill”
```

Docker then removes all containers with the label “maintainer” with a value of “bill.”

## How to Remove Docker Volumes

A [volume is used to store Docker data](https://phoenixnap.com/kb/docker-volumes).

It helps store and organize data outside containers in a way that it’s accessible to multiple containers.

1. Use the following command to generate a list of all the available Docker volumes:

```text
docker volume ls
```

![List Docker volumes.](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20720%2076'%3E%3C/svg%3E)

Take note of the **VOLUME NAME** you want to remove.

2. Then enter:

```text
docker volume rm VolumeName
```

Make sure to replace _**`VolumeName`**_ with the actual name you generated with the previous command.

If the volume is in use by an existing container, the system responds with an error. This means need to remove the container first.

## How to Remove Docker Networks

Docker networks allow different containers to communicate with each other freely while also preventing traffic from outside the network. This is typically done with a Docker bridge network.

The **`prune`** command removes all unused networks.

### Removing a Single Network

1. **Display a list of all existing Docker networks** with the command:

```text
docker network ls
```

2. Take note of the **NETWORK ID** – this is the identifier used to remove a specific network. Then, enter:

```text
docker network rm [networkID]
```

Replace **`[networkID]`** with the ID you captured from the first command.

You may receive an error message that says the network has active endpoints. That means that the network is currently in use by containers. You need to remove the containers that are using the network before you can remove the network.

## Remove All Unused Docker Objects

The **`prune`** command automatically removes all resources that aren’t associated with a container. This is a streamlined way to clean up unused images, containers, volumes, and networks.

In a terminal window, enter the following:

```text
docker system prune
```

Additional flags can be included:

* **`–a`** To include stopped containers and unused images
* **`–f`** Bypasses confirmation dialog
* **`––volumes`** Removes all unused volumes

Also, you can specify a single type of object to be removed, instead of the entire environment:

```text
docker container prune
```

```text
docker image prune
```

```text
docker volume prune
```

```text
docker network prune
```

Running **`docker system prune -a`** removes both unused and dangling images . Images used in a container, either currently running or exited, will NOT be affected.

## List all Docker Resources

Enter the following commands to display resources:

```text
docker container ls
```

```text
docker image ls
```

```text
docker volume ls
```

```text
docker network ls
```

```text
docker info
```

The above-mentioned lists number of containers, images, and information about the Docker installation. These commands can help you locate and identify resources that you want to keep, or that you want to delete.

The following flags can also be added:

* **`–a`** display all of a resource \(including the ones that are stopped\)
* **`–q`** or **`--quiet`** \(display only the numeric ID\)

During the development cycle of an application in Docker containers, it’s easy to use a lot of storage with old versions of images and containers.

In this guide, you learned the most **common commands for removing Docker images, containers, volumes, and networks**.

