# virtual-background

Provides virtual background support for linux clients. 

For use on Zoom or any other video conferencing app

Based on Ben Elder's post https://elder.dev/posts/open-source-virtual-background/

## install
`git clone git@github.com:realgt/virtual-background.git`

`docker build -t bodypix ./bodypix`
`docker build -t fakecam ./fakecam`

`docker network create --driver bridge fakecam`

```
docker run -d \
  --name=bodypix \
  --network=fakecam \
  --gpus=all --shm-size=1g --ulimit memlock=-1 --ulimit stack=67108864 \
  bodypix
```

```
docker run -d \
  --name=fakecam \
  --network=fakecam \
  -p 8080:8080 \
  -u "$$(id -u):$$(getent group video | cut -d: -f3)" \
  $$(find /dev -name 'video*' -printf "--device %p ") \
  fakecam
```
