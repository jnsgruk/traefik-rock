# Traefik OCI Image

This repo is used for testing [`rockcraft`](https://github.com/canonical/rockcraft). It provides a direct port of this [Dockerfile](https://github.com/jnsgruk/traefik-oci-image/blob/main/Dockerfile).

**WARNING**: This is an experiment to test `rockcraft` only. Do not use this image.

## Build steps

```bash
# Install some deps
$ sudo snap install --classic rockcraft --edge
$ sudo snap install skopeo --edge --devmode

# Pack the ROCK (optionally add --verbose)
$ rockcraft pack

# For the following commands, you must either use `sudo` or ensure your user is a member of the
# `docker` group, or the commands will fail.

# Import the ROCK into the local docker image cache
$ skopeo --insecure-policy copy oci-archive:traefik_2.6.3.rock docker-daemon:traefik:2.6.3

# Run the image, invoking Traefik as the traefik user (with debug logging)
$ docker run --rm \
      --entrypoint /usr/bin/traefik \
      --user traefik \
      --interactive \
      --tty \
      traefik:2.6.3 \
      -log.level=DEBUG
```

## Broken things

- [ ] Cannot set an entrypoint
- [ ] Cannot set labels
- [ ] Override the `go` plugin install to avoid clobbering `/bin`
