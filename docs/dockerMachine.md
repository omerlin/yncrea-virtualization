# Why Docker machine ?

Integrates this : https://docs.docker.com/machine/#whats-the-difference-between-docker-engine-and-docker-machine

## Installation of DockerMachine:

Follow: [Docker machine installation link](https://github.com/docker/machine/releases)

``` bash
$ if [[ ! -d "$HOME/bin" ]]; then mkdir -p "$HOME/bin"; fi && \
curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" && \
chmod +x "$HOME/bin/docker-machine.exe"
```

!!! Important
    To play the above script, you will need {==git-bash==}

