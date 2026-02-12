# Manually Build qBittorrent-nox Docker Image

This Dockerfile allows you to build a Docker Image containing qBittorrent-nox

## Prerequisites

`docker`
or
`podman (with podman-compose)`

This repository has only been tested with `podman`.

## Building Docker Image

Edit `docker-compose.yaml` file first.

Then run the following commands in this folder:
```shell
docker compose build
```
Consider removing the build layers afterwards. They are not needed for the final image and take up a few GB.


## Running container


* Using Docker Compose:
  ```shell
  docker compose up
  ```

* A few notes:
  * You can pass command-line arguments to `qbittorrent-nox` by appending them to the end of `docker run ...` command.
    If using Docker Compose, modify the `command:` array in docker-compose.yml.
  * By default the timezone in the container uses the default of Alpine Linux (which is most likely `UTC`).
    You can set the environment variable `TZ` to your preferred value.
  * You can change the User ID (UID) and Group ID (GID) of the `qbittorrent-nox` process by setting
    environment variables `PUID` and `PGID` respectively. By default they are both set to `1000`. \
    Note that you will need to remove `--read-only` flag (when using Docker) or set
    `read_only: false` (when using Docker Compose) as these settings are incompatible with each other.
  * You can set additional group ID (AGID) of the `qbittorrent-nox` process by setting the
    environment variable `PAGID`. For example: `10000,10001`, this will set the process to be in
    two (secondary) groups `10000` and `10001`. By default there is no additional group. \
    Note that you will need to remove `--read-only` flag (when using Docker) or set
    `read_only: false` (when using Docker Compose) as they are incompatible with it.
  * It is possible to set the umask of the `qbittorrent-nox` process by setting the
    environment variable `UMASK`. By default it uses the default from Alpine Linux.
  * You can list the compile-time Software Bill of Materials (sbom) with the following command:
    ```shell
    docker run --entrypoint /bin/cat --rm qbittorrentofficial/qbittorrent-nox:latest /sbom.txt
    ```

* Then you can login to qBittorrent-nox at: `http://<your_docker_host_address>:8080`
  * For older qBittorrent versions (< 4.6.1), the default username/password is: `admin/adminadmin`.
  * For newer qBittorrent versions (≥ 4.6.1), qBittorrent will generate a temporary password and print it to the console (via stdout).
    You need to use it to login. See the [announcement](https://www.qbittorrent.org/news#mon-nov-20th-2023---qbittorrent-v4.6.1-release). \
    If you don't have a console attached, you can run `docker logs qbittorrent-nox` to show the logs.

  After logging in, don't forget to change the password to something else! \
  To change it in WebUI: 'Tools' menu -> 'Options...' -> 'Web UI' tab -> 'Authentication'

## Stopping container

* Using Docker (not Docker Compose):
  ```shell
  docker stop -t 1800 qbittorrent-nox
  ```

* Using Docker Compose:
  ```shell
  docker compose down
  ```
