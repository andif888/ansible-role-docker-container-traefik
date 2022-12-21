# ansible-role-docker-container-traefik

Role to run Traefik in a docker container

## Table of content

- [Default Variables](#default-variables)
  - [docker_container_traefik_env](#docker_container_traefik_env)
  - [docker_container_traefik_image](#docker_container_traefik_image)
  - [docker_container_traefik_labels](#docker_container_traefik_labels)
  - [docker_container_traefik_letsencrypt_acme_json_file](#docker_container_traefik_letsencrypt_acme_json_file)
  - [docker_container_traefik_letsencrypt_acme_json_wait_for](#docker_container_traefik_letsencrypt_acme_json_wait_for)
  - [docker_container_traefik_name](#docker_container_traefik_name)
  - [docker_container_traefik_networks](#docker_container_traefik_networks)
  - [docker_container_traefik_ports](#docker_container_traefik_ports)
  - [docker_container_traefik_provider_config_template](#docker_container_traefik_provider_config_template)
  - [docker_container_traefik_restic_enable](#docker_container_traefik_restic_enable)
  - [docker_container_traefik_restic_retention](#docker_container_traefik_restic_retention)
  - [docker_container_traefik_restic_s3_bucket_name](#docker_container_traefik_restic_s3_bucket_name)
  - [docker_container_traefik_restic_s3_endpoint](#docker_container_traefik_restic_s3_endpoint)
  - [docker_container_traefik_restic_s3_repo](#docker_container_traefik_restic_s3_repo)
  - [docker_container_traefik_restic_s3_repo_access_key](#docker_container_traefik_restic_s3_repo_access_key)
  - [docker_container_traefik_restic_s3_repo_password](#docker_container_traefik_restic_s3_repo_password)
  - [docker_container_traefik_restic_s3_repo_secret_key](#docker_container_traefik_restic_s3_repo_secret_key)
  - [docker_container_traefik_restic_tag](#docker_container_traefik_restic_tag)
  - [docker_container_traefik_static_certificates](#docker_container_traefik_static_certificates)
  - [docker_container_traefik_volume_dir](#docker_container_traefik_volume_dir)
  - [docker_container_traefik_volumes](#docker_container_traefik_volumes)
  - [docker_image_traefik_name](#docker_image_traefik_name)
  - [docker_image_traefik_pull](#docker_image_traefik_pull)
  - [docker_network_traefik_name](#docker_network_traefik_name)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Default Variables

### docker_container_traefik_env

Dictionery of key,value pairs for docker environment variables to configure Traefik. https://doc.traefik.io/traefik/reference/static-configuration/env/

#### Default value

```YAML
docker_container_traefik_env:
  TRAEFIK_API: 'true'
  TRAEFIK_API_DASHBOARD: 'true'
  TRAEFIK_API_INSECURE: 'true'
  TRAEFIK_ENTRYPOINTS_HTTP: 'true'
  TRAEFIK_ENTRYPOINTS_HTTP_ADDRESS: :80
  TRAEFIK_ENTRYPOINTS_HTTPS: 'true'
  TRAEFIK_ENTRYPOINTS_HTTPS_ADDRESS: :443
  TRAEFIK_GLOBAL_CHECKNEWVERSION: 'false'
  TRAEFIK_GLOBAL_SENDANONYMOUSUSAGE: 'false'
  TRAEFIK_LOG: 'true'
  TRAEFIK_LOG_LEVEL: INFO
  TRAEFIK_PROVIDERS_DOCKER: 'true'
  TRAEFIK_PROVIDERS_DOCKER_EXPOSEDBYDEFAULT: 'false'
  TRAEFIK_PROVIDERS_DOCKER_NETWORK: '{{ docker_network_traefik_name }}'
  TRAEFIK_PROVIDERS_FILE_FILENAME: /etc/traefik/traefik.providers.file.toml
  TRAEFIK_PROVIDERS_FILE_WATCH: 'true'
  TRAEFIK_METRICS_PROMETHEUS: 'true'
```

### docker_container_traefik_image

Repository path and tag used to create the container. If an image is not found or pull is true, the image will be pulled from the registry. If no tag is included, latest will be used.

#### Default value

```YAML
docker_container_traefik_image: '{{ docker_image_traefik_name }}'
```

### docker_container_traefik_labels

Dictionary of key value pairs for container labels.

Example:

```yaml

docker_container_traefik_labels:

traefik.enable: "true"

```

#### Default value

```YAML
docker_container_traefik_labels: {}
```

### docker_container_traefik_letsencrypt_acme_json_file

letsencrypt acme.json file

#### Default value

```YAML
docker_container_traefik_letsencrypt_acme_json_file: '{{ docker_container_traefik_volume_dir
  }}/letsencrypt/acme.json'
```

### docker_container_traefik_letsencrypt_acme_json_wait_for

Pause setup process until letsencrypt acme.json is created

#### Default value

```YAML
docker_container_traefik_letsencrypt_acme_json_wait_for: false
```

### docker_container_traefik_name

Name for the container

#### Default value

```YAML
docker_container_traefik_name: traefik
```

### docker_container_traefik_networks

List of networks the container belongs to.

#### Default value

```YAML
docker_container_traefik_networks:
  - name: '{{ docker_network_traefik_name }}'
```

### docker_container_traefik_ports

List of ports to publish from the container to the host.

#### Default value

```YAML
docker_container_traefik_ports:
  - 80:80
  - 443:443
  - 8088:8080
```

### docker_container_traefik_provider_config_template

Path to provider template file

#### Default value

```YAML
docker_container_traefik_provider_config_template: templates/traefik.providers.file.toml.j2
```

### docker_container_traefik_restic_enable

Enable restic backup for the container's mounted volumes.

#### Default value

```YAML
docker_container_traefik_restic_enable: false
```

### docker_container_traefik_restic_retention

Retention settions for `restic forget` after the `restic backup`.

#### Default value

```YAML
docker_container_traefik_restic_retention:
  keep_last: 1
  keep_daily: 7
  keep_weekly: 4
```

### docker_container_traefik_restic_s3_bucket_name

Minio S3 bucket name for restic backup storage.

#### Default value

```YAML
docker_container_traefik_restic_s3_bucket_name: restic-{{ docker_container_traefik_name
  }}
```

### docker_container_traefik_restic_s3_endpoint

Minio S3 endpoint for restic backup storage.

Example:

```yaml

docker_container__base__restic_s3_endpoint: "https://minio.{{ dns_domain }}"

docker_container_traefik_restic_s3_endpoint: "{{ docker_container__base__restic_s3_endpoint }}"

```

#### Default value

```YAML
docker_container_traefik_restic_s3_endpoint: '{{ docker_container__base__restic_s3_endpoint
  }}'
```

### docker_container_traefik_restic_s3_repo

Minio S3 repo URL for restic backup storage.

#### Default value

```YAML
docker_container_traefik_restic_s3_repo: s3:{{ docker_container_traefik_restic_s3_endpoint
  }}/{{ docker_container_traefik_restic_s3_bucket_name }}
```

### docker_container_traefik_restic_s3_repo_access_key

Minio S3 repo access key for restic backup storage.

#### Default value

```YAML
docker_container_traefik_restic_s3_repo_access_key: '{{ docker_container__base__restic_s3_repo_access_key
  }}'
```

### docker_container_traefik_restic_s3_repo_password

Minio S3 repo password for restic backup storage.

#### Default value

```YAML
docker_container_traefik_restic_s3_repo_password: '{{ docker_container__base__restic_s3_repo_password
  }}'
```

### docker_container_traefik_restic_s3_repo_secret_key

Minio S3 repo secret key for restic backup storage.

#### Default value

```YAML
docker_container_traefik_restic_s3_repo_secret_key: '{{ docker_container__base__restic_s3_repo_secret_key
  }}'
```

### docker_container_traefik_restic_tag

Tag for the `restic backup` command

#### Default value

```YAML
docker_container_traefik_restic_tag: '{{ docker_container_traefik_name }}'
```

### docker_container_traefik_static_certificates

Static certificates used by Traefik.

Example:

```yaml

docker_container_traefik_static_certificates:

- { cert_file: "files/acme1_org.cer", key_file: "files/acme1_org.pem" }

- { cert_file: "files/acme2_org.cer", key_file: "files/acme1_org.pem" }

```

#### Default value

```YAML
docker_container_traefik_static_certificates: []
```

### docker_container_traefik_volume_dir

Volume mount host directory, where Treafik config files are stored.

#### Default value

```YAML
docker_container_traefik_volume_dir: '{{ docker_container__base__volume_dir }}/{{
  docker_container_traefik_name }}'
```

### docker_container_traefik_volumes

List of volumes to mount within the container.

#### Default value

```YAML
docker_container_traefik_volumes:
  - /var/run/docker.sock:/var/run/docker.sock:ro
  - '{{ docker_container_traefik_volume_dir }}/config:/etc/traefik'
  - '{{ docker_container_traefik_volume_dir }}/letsencrypt:/letsencrypt'
```

### docker_image_traefik_name

Repository path and tag for the container image.

#### Default value

```YAML
docker_image_traefik_name: traefik:v2.9
```

### docker_image_traefik_pull

Indicate to always pull the docker image.

#### Default value

```YAML
docker_image_traefik_pull: no
```

### docker_network_traefik_name

Name of the docker network created for Traefik.

#### Default value

```YAML
docker_network_traefik_name: '{{ docker_container_traefik_name }}_backend'
```

## Discovered Tags

**_docker-container-backup-all_**\
&emsp;Backup all containers' volume mounts.

**_docker-container-backup-init-all_**\
&emsp;Run init backup task for all container.

**_docker-container-backup-init-traefik_**\
&emsp;Run init backup task for Traefik if restic is enabled.

**_docker-container-backup-list-all_**\
&emsp;List all containers' backups.

**_docker-container-backup-list-traefik_**\
&emsp;List Traefik backups.

**_docker-container-backup-traefik_**\
&emsp;Backup Traefik volume mounts.

**_docker-container-prereq-all_**\
&emsp;Ensure all pre-requisites are installed

**_docker-container-prereq-traefik_**\
&emsp;Ensure all pre-requisites for Traefik are installed

**_docker-container-purge-all_**\
&emsp;Remove all containers and delete volume mounts.

**_docker-container-purge-traefik_**\
&emsp;Remove Traefik and delete volume mounts.

**_docker-container-remove-all_**\
&emsp;Remove all containers.

**_docker-container-remove-traefik_**\
&emsp;Remove Traefik.

**_docker-container-restore-all_**\
&emsp;Run restic restore for all restic enabled containers.

**_docker-container-restore-traefik_**\
&emsp;Run restic restore for Traefik if restic is enabled.

**_docker-container-setup-all_**\
&emsp;Run setup task for all containers.

**_docker-container-setup-traefik_**\
&emsp;Run setup task for Traefik.

**_never_**


## Dependencies

None.

## License

license (GPL-2.0-or-later, MIT, etc)

## Author

andif888
