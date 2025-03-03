![](xTeVe_logo.PNG)

# xteve-streamlink Docker

A debian xTeVe container with streamlink!
Forked from [alturismo/xteve](https://github.com/alturismo/xteve)
Credits to those who developed xTeVe, streamlink, as well as **alturismo** who made the original container

- [xteve-streamlink Docker](#xteve-streamlink-docker)
  - [Environment Variables](#environment-variables)
  - [Docker compose](#docker-compose)
  - [Docker run](#docker-run)
  - [Using streamlink in xTeVe](#using-streamlink-in-xteve)


## Environment Variables

| Name | Description |
|---|---|
| TZ | Timezone (eg. America/Chicago)  |
| I_FFMPEG | if 1, install ffmpeg (default: 0) |
| I_STREAMLINK | if 1, install streamlink (default: 1) |


## Docker compose

```yaml
version: '3'

services:
  xteve:
    container_name: xteve-streamlink
    image: ghcr.io/seansusmilch/xteve-streamlink
    ports:
      - 34400:34400
    environment:
      TZ: 'America/Chicago'
      I_FFMPEG: 0
      I_STREAMLINK: 1
    volumes:
      - .\mnt\config:/config
      - .\mnt\root:/root/.xteve
      - .\mnt\tmp:/tmp/xteve
```

## Docker run

```bash
docker run -d \
  --name=xteve \
  --net=host \
  --log-opt max-size=10m \
  --log-opt max-file=3 \
  -e TZ="Europe/Berlin" \
  -v /mnt/user/appdata/xteve/:/root/.xteve:rw \
  -v /mnt/user/appdata/xteve/_config:/config:rw \
  -v /tmp/xteve/:/tmp/xteve:rw \
  ghcr.io/seansusmilch/xteve-streamlink
```

## Using streamlink in xTeVe

xTeVe doesn't officially support streamlink when connecting to streams, however theres a bit of a hacky way.

See this github discussion [here](https://github.com/streamlink/streamlink/discussions/3430#discussioncomment-234211)

I've had a better experience adding a few more arguments in there, making the final arg list below

```bash
--stdout [URL] --default-stream best --stream-segment-threads 5 --retry-streams 1 --retry-open 3 --stream-types hls
```
