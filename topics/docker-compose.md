[//]: # (title: Docker Compose)
[//]: # (auxiliary-id: Docker Compose)

The _Docker Compose_ [build runner](build-runner.md) allows starting [Docker Compose](https://docs.docker.com/compose/) build services and shutting them down at the end of the build. With this runner, you can run multi-container Docker apps.

## Common Settings

This runner is a part of the TeamCity-Docker integration toolset. Refer to [this page](integrating-teamcity-with-docker.md) for information on software requirements, supported environments, and other common aspects of this integration.

Available step execution policies are described [here](configuring-build-steps.md#Execution+Policy).

## Docker Compose Settings

The Docker Compose runner supports one or multiple [Docker Compose YAML file(s)](https://docs.docker.com/compose/compose-file/compose-file-v2/) with a description of the services to be used during the build. The path to the `docker-compose.yml` file(s) should be relative to the [build checkout directory](build-checkout-directory.md). When specifying multiple files, separate them with a space.

A [build agent](build-agent.md) will execute the following `docker-compose` commands during the build:

```Shell

# The commands are executed on the current working directory, where the docker-compose file resides.
docker-compose -f <docker-compose.yml> [-f <docker-compose2.yml>] up -d

# At the end of the build, for each Docker Compose build step the build agent will run:
docker-compose -f <docker-compose.yml> [-f <docker-compose2.yml>] down -v

```

If the __pull image explicitly__ option is enabled, `docker-compose pull` will be run before the `docker-compose up` command.

When using Docker Compose with images which support [HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck), TeamCity will wait for the `healthy` status of all containers that support this parameter.

If the start of Docker Compose was successful, the TeamCity agent will register the `TEAMCITY_DOCKER_NETWORK` environment variable containing the name of the Docker Compose default network. This network will be passed transparently to the [Docker Wrapper](docker-wrapper.md) when it is used in some build runners.

<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-docker.md">Integrating TeamCity with Docker</a>
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-support.md">Docker Support feature</a>
            <a href="docker.md">Docker runner</a>
            <a href="docker-wrapper.md">Docker Wrapper extension</a>
        </category>
</seealso>