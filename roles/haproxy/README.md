# Ansible Role: Docker

An Ansible Role that installs [Docker](https://www.docker.com) on
Centos Linux.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see
`defaults/main.yml`):

    docker_package: "docker-ce"
    docker_package_state: present

You can also specify a specific version of Docker to install using the
distribution-specific format: Red Hat/CentOS: `docker-ce-<VERSION>`.

You can control whether the package is installed, uninstalled, or at
the latest version by setting `docker_package_state` to `present`,
`absent`, or `latest`, respectively. Note that the Docker daemon will
be automatically restarted if the Docker package is updated. This is a
side effect of flushing all handlers (running any of the handlers that
have been notified by this and any other role up to this point in the
play).

    docker_service_state: started
    docker_service_enabled: true
    docker_restart_handler_state: restarted

Variables to control the state of the `docker` service, and whether it
should start on boot. If you're installing Docker inside a Docker
container without systemd or sysvinit, you should set these to
`stopped` and set the enabled variable to `no`.

    docker_install_compose: true
    docker_compose_version: "1.24.1"
    docker_compose_path: /usr/local/bin/docker-compose

Docker Compose installation options.

    docker_users:
      - user1
      - user2

A list of system users to be added to the `docker` group (so they can
use Docker on the server).

## Proxy settings

This role reads a variable/fact `proxy_config` - when it is set to
something like the following:

    proxy_config:
      http_proxy: "http://my_proxy:port"
      https_proxy: "http://my_proxy:port"
      HTTP_PROXY: "my_proxy:port"
      HTTPS_PROXY: "my_proxy:port"
      NO_PROXY: "localhost,127.0.*"

## Dependencies

None.
