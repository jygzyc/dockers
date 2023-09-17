# Introduction

Frida patch version

# Usage

## Build Docker

```bash
docker build . -t frida:v1
```

## Run Docker

```bash
docker run -it -v <frida-project-dir>:/frida-env --name frida-dev frida:v1 /bin/bash
```

Rerun bash

```bash
docker start frida-dev && docker exec -it frida-dev /bin/bash
```

Remove container

```bash
docker container rm frida-dev -f
```

Remove image

```bash
docker image rm frida -f
```

## Then

Pull

```bash
git clone --recurse-submodules https://github.com/frida/frida
```

Build
 
```bash
./frida-scripts/build_android_arm64.sh
```

Patch

```bash
./frida-scripts/patch.sh
```