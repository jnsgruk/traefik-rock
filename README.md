# Traefik OCI Image

This repo is used for testing [`rockcraft`](https://github.com/canonical/rockcraft). It provides a direct port of this [Dockerfile](https://github.com/jnsgruk/traefik-oci-image/blob/main/Dockerfile).

**WARNING**: This is an experiment to test `rockcraft` only. Do not use this image.

## Build steps

```bash
# Install some deps
$ sudo snap install --classic rockcraft --edge
$ sudo snap install skopeo --edge --devmode

# Pack the ROCK
$ rockcraft pack

# Import the ROCK into the local docker image cache
$ sudo skopeo --insecure-policy copy oci-archive:traefik_2.6.3.rock docker-daemon:traefik:2.6.3

# Run the image, invoking Traefik and showing the version
$ sudo docker run --rm --entrypoint /usr/bin/traefik -it traefik:2.6.3 version
```

## Broken things

- [ ] There is no `traefik` user
- [ ] `/etc/traefik` does not get created
- [ ] Cannot set an entrypoint
