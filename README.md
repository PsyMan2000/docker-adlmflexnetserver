# ADLMFlexNetServer

[![Docker Stars](https://img.shields.io/docker/stars/haysclark/adlmflexnetserver.svg?maxAge=2592000)](https://hub.docker.com/r/haysclark/adlmflexnetserver/) [![](https://img.shields.io/docker/pulls/haysclark/adlmflexnetserver.svg)](https://hub.docker.com/r/haysclark/adlmflexnetserver "DockerHub") [![license](https://img.shields.io/github/license/mashape/apistatus.svg)]() [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com) [![Code of conduct](https://img.shields.io/badge/code%20of-conduct-ff69b4.svg)](https://github.com/haysclark/adlmflexnetserver/blob/main/CODE_OF_CONDUCT.md) [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release#-semantic-release)

> Unofficial Autodesk License Manager FLEXlm® Network Server container.

## Usage

> To use this container, simply treat it as if you were calling _lmgrd_ directly. The only caveat is that the container **ALWAYS** appends the `-z` flag when executing _lmgrd_ internally so to ensure it runs in the Foreground; otherwise, Docker believes the command has finished executing, and the container is immediately stopped.

A typical usage example, where a single license file, `adsk_server.lic`, is saved to a local directory. This directory's path, `<local-license-path>`, is them mapped to the containers **/var/flexlm** directory, and is finally referenced as if calling as _lmgrd_ directly: `-c /var/flexlm/adsk_server.lic`.

```bash
docker run -d --mac-address=<license-mac-address> \
-h <license-hostname> \
-v <local-license-path>:/var/flexlm/adsk_server.lic:ro \
-p 2080:2080 -p 27000-27009:27000-27009 \
haysclark/adlmflexnetserver \
-c /var/flexlm/adsk_server.lic
```

Alternatively, you can map a local folder containing multiple keys to the **/var/flexlm** directory as well.

```bash
docker run -d --mac-address=<license-mac-address> \
-h <license-hostname> \
-v <local-license-folder-path>:/var/flexlm \
-p 2080:2080 -p 27000-27009:27000-27009 \
haysclark/adlmflexnetserver \
-c /var/flexlm/license_file_1:/var/flexlm/license_file_2
```

### Logging

Docker's built-in logging functionality will collect the stdout/stderr generated by _lmgrd_.

```bash
docker logs <container-id>
```

Thus it's recommended you do **NOT** use the `-l` flag to log to a file, doing so will cause your Docker logs to be empty. Additionally, avoid using the `-t` flag when using Docker's `run` command, enabling TTL support will cause extra line breaks in your Docker logs.

## Troubleshooting

If you are unsure if the server is running correctly, you can log into the container.

```bash
docker exec -it <container-id> /bin/bash
```

Once in bash run:

```bash
lmutil lmstat -a -c <license-path>
```

## Image Variants

### haysclark/adlmflexnetserver:<version>

#### NLM

| Version   | Description                                                           |
| --------- | --------------------------------------------------------------------- |
| N/A       | Uses NLM 11.18.00 (required for 2023 products / backwards compatible  |
| >= v1.0.1 | Uses NLM 11.16.2.0 (required for 2020 products / backwards compatible |
| v1.0.0    | Uses NLM 11.13.1.2 (support pre-2020 products)                        |

#### CentOS

| Version   | Description                         |
| --------- | ----------------------------------- |
| N/A       | Uses Alma Linux 8.6 (EOL 2029)
| >= v1.1.0 | Uses CentOS7 (EOL on June 2024)     |
| < v1.1.0  | Uses CentOS6 (EOL on November 2020) |

## Building a custom Docker image

The base container is intended to support a wide range of cases; however, some _build arguments_ have been included to allow easy access to aspects of the build process.

### Build Arguments

The following build arguments have been exposed.

| Name      | Required | Description                                       |
| --------- | -------- | ------------------------------------------------- |
| NLM_URL   | no       | URL to the NLM (Network License Manager) .gz file |
| TEMP_PATH | no       | temporary file path for installation              |

Usage example:

```bash
docker build --build-arg NLM_URL=<nlm-url> .
```

## Resources

- [Official Docs](https://knowledge.autodesk.com/search-result/caas/downloads/content/autodesk-network-license-manager-for-linux.html)

## Supports

Applies to Autodesk Nastran 2015, Autodesk Nastran 2016, Autodesk Nastran 2017, Infrastructure Map Server 2014, Infrastructure Map Server 2015, Infrastructure Map Server 2016, Infrastructure Map Server 2017, Maya 2014, Maya 2015, Maya 2016, Maya 2017, Moldflow Insight 2015, Moldflow Insight 2016, Moldflow Insight 2017, MotionBuilder 2014, MotionBuilder 2015, MotionBuilder 2016, MotionBuilder 2017, Mudbox 2014, Mudbox 2015, Mudbox 2016, Mudbox 2017, Softimage 2014, Softimage 2015, Softimage 2016, VRED Design 2014, VRED Products 2017 and more.
