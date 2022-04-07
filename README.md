# Traefik OCI Image

This repo is used for testing [`rockcraft`](https://github.com/canonical/rockcraft). It provides a
direct port of this [Dockerfile](https://github.com/jnsgruk/traefik-oci-image/blob/main/Dockerfile).

Critically, this image includes [pebble](https://github.com/canonical/pebble) to start and manage
Traefik.

**WARNING**: This is an experiment to test `rockcraft` only. Do not use this image.

## Build steps

```bash
# Install some deps
$ sudo snap install rockcraft --classic --edge
$ sudo snap install skopeo --edge --devmode

# Pack the ROCK (optionally add --verbose)
$ rockcraft pack

# For the following commands, you must either use `sudo` or ensure your user is a member of the
# `docker` group, or the commands will fail.

# Import the ROCK into the local docker image cache
$ skopeo --insecure-policy copy oci-archive:traefik_2.6.3.rock docker-daemon:traefik:2.6.3

# Run the image, invoking Traefik through pebble
$ docker run --rm --entrypoint pebble -it traefik:2.6.3 run -v
```

## Broken things

- [ ] Start traefik as non-root user using Pebble
- [ ] Cannot set an entrypoint
- [ ] Cannot set labels
- [ ] Override the `go` plugin install to avoid clobbering `/bin`
