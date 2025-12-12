## Step 1: Create a named volume ####

```shell
docker volume create so1-etc
docker volume create so1-var
```

When the volume is mounted, the data will persist after the container exits. If a container has exited and restarted, but no data shows up, check the volume definition and verify that the container did not create a new volume or that the volume mounted is in the same location.

## Step 2: Start the service ####
In the same directory as `docker-compose.yml`, run the following command to start the service:

```shell
docker-compose up
```

## Viewing the contents of the volume ####
To view the /etc directory outside of the container, run one or both of the commands
```shell
docker volume inspect so1-etc
```
The output of that command should list the directory associated with the volume mount.

## To view the logs from the container created above, 

run:

```bash
$ docker logs -f so1
```

## To enter the container and run Splunk CLI commands, run:
```bash
# Defaults to the user "ansible"
docker exec -it so1 /bin/bash

# Run shell as the user "splunk"
docker exec -u splunk -it so1 bash
```

## To enable TCP 10514 for listening, run:
```bash
docker exec -u splunk so1 /opt/splunk/bin/splunk add tcp 10514 \
    -sourcetype syslog -resolvehost true \
    -auth "admin:${SPLUNK_PASSWORD}"
```

## To install an app, run:
```bash
docker exec -u splunk so1 /opt/splunk/bin/splunk install \
	/path/to/app.tar -auth "admin:${SPLUNK_PASSWORD}"

# Alternatively, apps can be installed at Docker run-time
docker run -e SPLUNK_APPS_URL=http://web/app.tgz ...
```


## Vérifier l'état de splunk : 

```bash
docker exec -it  -u splunk so1 /opt/splunk/bin/splunk status
```


# src

See [Deploy and run Splunk Enterprise inside a Docker container](https://docs.splunk.com/Documentation/Splunk/latest/Installation/DeployandrunSplunkEnterpriseinsideDockercontainers) for more information.

Visit the [Docker-Splunk documentation](https://splunk.github.io/docker-splunk/) page for full usage instructions, including installation, examples, and advanced deployment scenarios.

- https://help.splunk.com/en/splunk-enterprise/administer/install-and-upgrade/10.0/install-splunk-enterprise-in-virtual-and-containerized-environments/deploy-and-run-splunk-enterprise-inside-a-docker-container#id_80796007_3ae3_4262_a69a_1be2720d44a5__Deploy_and_run_Splunk_Enterprise_inside_a_Docker_container