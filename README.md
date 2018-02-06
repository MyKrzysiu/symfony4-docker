# symfony4-docker
Docker for Symfony 4 application (with nginx, php 7.2, composer, mysql and elk - elasticsearch logstash and kibana)

Optimized for macOS High Sierra

## Requirements

1. [nginx-proxy](https://github.com/jwilder/nginx-proxy)

## Installation

1. Create a `.env` from the `.env.dist` file. Adapt it according to your symfony application

    ```bash
    cp .env.dist .env
    ```

2. Build/run containers with detached mode

    ```bash
    $ docker-compose up -d
    ```

