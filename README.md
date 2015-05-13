
# Table of Contents
- [Introduction](#introduction)
    - [Version](#version)
- [Hardware Requirements](#hardware-requirements)
    - [Memory](#memory)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
  - [Avaible Configuration Parameters](#avaible-configuration-parameters)
  - [Advance configuration](#advance-configuration)
- [References](#references)

# Introduction

Dockerfile to build a ActiveMQ container image.

## Version

Current Version: **5.11.1**

# Hardware Requirements

## Memory

- 512MB is too little memory, i think is to use ActiveMQ on test environment
- **1GB** is the **standard** memory size

You can set the memory that you need :

```bash
docker run ... \
	-e 'ACTIVEMQ_MIN_MEMORY=512' \
	-e 'ACTIVEMQ_MAX_MEMORY=2048'

```
This sample lauch ActiveMQ in docker with 512 MB of memory, and then ACtiveMQ can take 2048 MB of max memory

# Installation

Pull the image from the docker index. This is the recommended method of installation as it is easier to update image. These builds are performed by the **Docker Trusted Build** service.

```bash
docker pull parana/soma-mq:5.10.0
```

You can also pull the `latest` tag which is built from the repository *HEAD*

```bash
docker pull parana/soma-mq:latest
```

Alternately you can build the image locally.

```bash
git clone https://github.com/joao-parana/soma-mq.git
cd soma-mq
docker build --tag="$USER/soma-mq" .
```

# Quick Start

You can launch the image using the docker command line :

- **For test purpose :**

```bash
docker run --name='somamq' -it --rm \
       parana/soma-mq
```
The account admin is "admin" and password is "admin". All settings is the default ActiveMQ's settings.

- **For production purpose :**

```bash
docker run --name='soma-mq' -d \
        -e 'ACTIVEMQ_ADMIN_PASSWORD=password' \
        -e 'ACTIVEMQ_USER_LOGIN=login' \
        -e 'ACTIVEMQ_USER_PASSWORD=password' \
        -e 'ACTIVEMQ_WRITE_LOGIN=producer_login' \
        -e 'ACTIVEMQ_WRITE_PASSWORD=producer_password' \
        -e 'ACTIVEMQ_READ_LOGIN=consumer_login' \
        -e 'ACTIVEMQ_READ_PASSWORD=consumer_password' \
        -e 'ACTIVEMQ_JMX_LOGIN=jmx_login' \
        -e 'ACTIVEMQ_JMX_PASSWORD=jmx_password' \
        -v /data/activemq:/data/activemq \
        -v /var/log/activemq:/var/log/activemq \
      parana/soma-mq
```


Or you can use [fig](http://www.fig.sh/). Assuming you have fig installed,

```bash
wget https://raw.githubusercontent.com/joao-parana/soma-mq/master/fig.yml
fig up
```

# Configuration

## Avaible Configuration Parameters

*Please refer the docker run command options for the `--env-file` flag where you can specify all required environment variables in a single file. This will save you from writing a potentially long docker run command. Alternately you can use fig.*

Below is the complete list of available options that can be used to customize your gitlab installation.

- **ACTIVEMQ_NAME**: The hostname of ActiveMQ server. Default to `localhost`
- **ACTIVEMQ_LOGLEVEL**: The log level. Default to `INFO`
- **ACTIVEMQ_PENDING_MESSAGE_LIMIT**: It is used to prevent slow topic consumers to block producers and affect other consumers by limiting the number of messages that are retained. Default to `1000`
- **ACTIVEMQ_STORAGE_USAGE**: The maximum amount of space storage the broker will use before disabling caching and/or slowing down producers. Default to `100 gb`
- **ACTIVEMQ_TEMP_USAGE**: The maximum amount of space temp the broker will use before disabling caching and/or slowing down producers. Default to `50 gb`
- **ACTIVEMQ_MAX_CONNECTION**: It's DOS protection. It limit concurrent connections. Default to `1000`
- **ACTIVEMQ_FRAME_SIZE**: It's DOS protection. It limit the frame size. Default to `104857600` (100MB)
- **ACTIVEMQ_MIN_MEMORY**: The init memory in MB that ActiveMQ take when start (it's like XMS). Default to `128` (128 MB)
- **ACTIVEMQ_MAX_MEMORY**: The max memory in MB that ActiveMQ can take (it's like XMX). Default to `1024` (1024 MB)

- **ACTIVEMQ_ADMIN_LOGIN**: The login for admin account (broker and web console). Default to `admin` 
- **ACTIVEMQ_ADMIN_PASSWORD**: The password for admin account. Default to `admin`
- **ACTIVEMQ_USER_LOGIN**: The login to access on web console with user role (no right on broker). Default to `user`
- **ACTIVEMQ_USER_PASSWORD**: The password for user account. Default to `user`
- **ACTIVEMQ_READ_LOGIN**: The login to access with read only role on all queues and topics.
- **ACTIVEMQ_READ_PASSWORD**: The password for read account.
- **ACTIVEMQ_WRITE_LOGIN**: The login to access with write role on all queues and topics.
- **ACTIVEMQ_WRITE_PASSWORD**: The password for write account.
- **ACTIVEMQ_OWNER_LOGIN**: The login to access with admin role on all queues and topics.
- **ACTIVEMQ_OWNER_PASSWORD**: The password for owner account.
- **ACTIVEMQ_JMX_LOGIN**: The login to access with read / write role on JMX. Default to `admin`
- **ACTIVEMQ_JMX_PASSWORD**: The password for JMX account. Default to `activemq`

- **ACTIVEMQ_STATIC_TOPICS**: The list of topics separated by comma witch is created when ActiveMQ start.
- **ACTIVEMQ_STATIC_QUEUES**: The list of queues separated by comma witch is created when ActiveMQ start.


## Advance configuration

For advance configuration, the best way is to read ActiveMQ documentation and created your own setting file like activemq.xml.
Next, you can mount it when you run this image or you can create your own image (base on this image) and include your specifics config file.
