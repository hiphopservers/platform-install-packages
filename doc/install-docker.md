# Installing Kaltura Docker container 
This guide describes docker container installation of an all-in-one Kaltura server and applies to all docker engines on linux, windows and mac.

[Kaltura Inc.](http://corp.kaltura.com) also provides commercial solutions and services including pro-active platform monitoring, applications, SLA, 24/7 support and professional services. If you're looking for a commercially supported video platform  with integrations to commercial encoders, streaming servers, eCDN, DRM and more - Start a [Free Trial of the Kaltura.com Hosted Platform](http://corp.kaltura.com/free-trial) or learn more about [Kaltura' Commercial OnPrem Edition™](http://corp.kaltura.com/Deployment-Options/Kaltura-On-Prem-Edition). For existing RPM based users, Kaltura offers commercial upgrade options.

#### Table of Contents

[Prerequites](install-docker.md#prerequites)

[Step-by-step Installation](install-docker.md#step-by-step-installation)

[Unattended Installation](install-docker.md#unattended-installation)

## Prerequites

Installed [Docker Engine](https://docs.docker.com/engine/installation).

## Step-by-step Installation

### Pre-Install notes
* This install guide assumes that you ran kaltura/server container.
* When installing, you will be prompted for each server's resolvable hostname. Note that it is crucial that all host names will be resolvable by other servers in the cluster (and outside the cluster for front machines). Before installing, verify that your /etc/hosts file is properly configured and that all Kaltura server hostnames are resolvable in your network.

### Running the image
Kaltura requires certain ports to be open for proper operation. [See the list of required open ports](kaltura-required-ports.md).
The container already documented to expose 80, 443 and 1935 ports, make sure to run it using -p option to enable access to these ports.

### Start Kaltura container
```bash
docker run -d --name=kaltura -p 80:80 -p 443:443 -p 1935:1935 -p 88:88 -p 8443:8443 kaltura/server
```

### Start of Kaltura Configuration

```bash
# docker exec -i -t kaltura /root/install/install.sh MYSQL_ROOT_PASSWD_HERE
``` 

Your install will now automatically perform all install tasks.
[See more details about the configuration process](install-kaltura-redhat-based.md#start-of-kaltura-configuration).

**Your Kaltura installation is now complete.**

## Unattended Installation
All Kaltura scripts accept an answer file as their first argument.
In order to preform an unattended [silent] install:

 - Create **config.ans** file according to [template](kaltura.template.ans).
 - Copy to file into the container: 
```bash
# docker cp config.ans kaltura:/root/install/
``` 
 - Run the installer, it will automatically search for answers file in this path.
```
# docker exec -i -t kaltura /root/install/install.sh
```

**Note:** answer files contain a mysql root password param called SUPER_USER_PASSWD. Therefore, do NOT pass a MySQL root passwd as an argument when performing an unattended installation.

