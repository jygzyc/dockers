# xmake dev env

## build image

```bash
docker buildx build . -t xmake:v1
```

## Running

```bash
docker run -it -v <your-project-dir>:/project --name xmake-dev xmake:v1 /bin/bash
```

Rerun bash

```bash
docker start xmake-dev && docker exec -it xmake-dev /bin/bash
```

Remove container

```bash
docker container rm xmake-dev -f
```

Remove image

```bash
docker image rm xmake -f
```

## Compile

```bash
# Run in docker
xmake f -p android -a arm64-v8a -m debug # Compiling Android arm64-v8a ELF file

xmake # build
```