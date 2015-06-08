# IBM® MQ Light
![IBM MQ Light logo](MQLightForDocker.png)
## Overview

This image contains [IBM MQ Light](https://developer.ibm.com/messaging/mq-light/).  The Dockerfile for this image can be found on the [ibm-messaging GitHub](https://github.com/ibm-messaging/mqlight-docker/blob/master/early-access/Dockerfile).

## Usage

In order to use the image, it is necessary to accept the terms of the MQ Light license.  This is achieved by specifying the environment variable `LICENSE` equal to `accept` when running the image.  You can also view the license terms by setting this variable to `view`. Failure to set the variable will result in the termination of the container with a usage statement.

The following command creates and starts an MQ Light container listening on the host's IP address on port 5672 for AMQP messaging, and 9180 for the web UI.  MQ Light's persistent message data and logs are written to a Docker volume in `/var/mqlight` - in this example, this directory in the container is mapped to the `/var/example` directory on the host:

```sh
sudo docker run \
  --env LICENSE=accept \
  --volume /var/example:/var/mqlight \
  --publish 5672:5672 \
  --publish 9180:9180 \
  --detach \
  ibmimages/mqlight:early-access
```

The following command creates and starts an MQ Light container with security enabled.  Note that the mapped ports have changed to be the secure ports.

```sh
sudo docker run \
  --env LICENSE=accept \
  --env MQLIGHT_USER=admin \
  --env MQLIGHT_PASSWORD=changeme \
  --env MQLIGHT_TLS_PASSPHRASE=changeme2 \
  --env MQLIGHT_TLS_KEYSTORE=/etc/ssl/selfsigned.p12 \
  --volume /path/on/host/selfsigned.p12:/etc/ssl/selfsigned.p12 \
  --volume /var/example:/var/mqlight \
  --publish 5671:5671 \
  --publish 9181:9181 \
  --detach \
  ibmimages/mqlight:early-access
```

See the [MQ Light security tutorial](https://developer.ibm.com/messaging/mq-light/docs/security-tutorial/) for more details on how to create a key store.

**Note:** because MQ Light uses memory-mapped files you cannot map `/var/mqlight` to a vboxsf file system due to [VirtualBox defect 819](https://www.virtualbox.org/ticket/819). For example, the `/Users` directory is typically a vboxsf file system when running under boot2docker on Mac OS X.

## Configuration

The following environment variables can be used to configure MQ Light:

* **LICENSE** - Set to "view" to view the license terms, or "accept" to accept the license terms.
* **MQLIGHT_USER**  | Sets the administrative user name.  Must be used in conjunction with MQLIGHT_PASSWORD.
* **MQLIGHT_PASSWORD** | Sets the administrative user password.  Must be used in conjunction with MQLIGHT_USER.
* **MQLIGHT_TLS_KEYSTORE** | Enables SSL/TLS security, and sets the PKCS#12 key store file path.  Note that you need to make the key store file available inside your Docker container - for example, you can add a volume by adding `-v /path/on/host/my.p12:/path/in/container/my.p12` when you invoke `docker run`.
* **MQLIGHT_TLS_PASSPHRASE** | Sets the passphrase for the key store file, for SSL/TLS security

See the [MQ Light documentation](https://developer.ibm.com/messaging/mq-light/docs/) for more details about how to use MQ Light.

## Issues and contributions

For issues relating specifically to this Docker image, please use the [GitHub issue tracker](https://github.com/ibm-messaging/mqlight-docker/issues). For more general issue relating to IBM MQ Light you can [get help](https://developer.ibm.com/answers/?community=messaging) through the Messaging community. If you do submit a Pull Request related to this Docker image, please indicate in the Pull Request that you accept and agree to be bound by the terms of the [IBM Contributor License Agreement](CLA.md).

## License

The Dockerfile and associated scripts are licensed under the [Apache License 2.0](LICENSE). MQ Light is licensed under the IBM International License Agreement for Non-Warranted Programs. This license may be viewed from the image using the `LICENSE=view` environment variable as described above or may be found [online](http://www14.software.ibm.com/cgi-bin/weblap/lap.pl?li_formnum=L-ACRR-9MKEUW). Note that this license does not permit further distribution.
