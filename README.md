> [!NOTE]
> See my updated examples on how to use GitHub Actions to build and push to Docker Hub. Just search for my repositories beginning with [GitHub-Actions-to-Docker-Hub-\*](https://github.com/dersimn?tab=repositories&q=GitHub-Actions-to-Docker-Hub&type=&language=&sort=).


# Automated build of multi-arch Docker Images

See older workflows in [HelloARM](https://github.com/dersimn/HelloARM) repository, however the following is 'state of the art' as of 2022:

## Build locally

With the new `buildx` command, you can build multi-arch Images by just:

    docker buildx create --name mybuilder

    docker buildx use mybuilder
    docker buildx build \
        --platform \
            linux/amd64,\
            linux/arm/v7 \
        -t dersimn/helloarm \
        -t dersimn/helloarm:2 \
        -t dersimn/helloarm:2.3
        -t dersimn/helloarm:2.3.4 \
        --push .

## Using GitHub Workflow

You can use GitHub Workflows to trigger the exact same `buildx`-build on every pushed commit and even let GitHub tag your Docker Images for you using [semver tags](https://github.com/crazy-max/ghaction-docker-meta#handle-semver-tag).

With this method your useres can run

    docker run dersimn/helloarm-github-workflow
    docker run dersimn/helloarm-github-workflow:2
    docker run dersimn/helloarm-github-workflow:2.3
    docker run dersimn/helloarm-github-workflow:2.3.4
    docker run dersimn/helloarm-github-workflow:latest
    docker run dersimn/helloarm-github-workflow:<branch name>

on any platform, Docker will then pull the right image (amd64/arm/…) for you.

### Usage

#### Provide Login Information for Docker Hub

- In GitHub go to: Settings > Secrets
- Create
    - `DOCKERHUB_USERNAME` = ...
    - `DOCKERHUB_TOKEN` = ...

Generate a Token on <https://hub.docker.com> > Account Settings > Security > Access Tokens > New Access Token
