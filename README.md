c7r - c\[ontaine\]r
==================================

**stupid simple docker container build/push**

Features
----------------------------------------------

* Just some syntactic sugar
* Build docker images using the directory name as image tag
* Push images to registry using a predefined tag stored in the image directory
* Rebuild images (no-cache option set)
* Provide additional build params via file

Directory structure
----------------------------------------------

**Config Files**

* `.repository` - simply 1-line text file containing the absolute image url
* `.c7r` - additional build arguments used for docker

File: `.repository`

```raw
https://myregistry.example.org/core/busybox
```

File: `.c7r`

```raw
BUILD_OPT="--target stage1 --quiet --build-arg X=hello"
```

**Image directory**

A simple image directory used to build the image

```raw
busybox
├── Dockerfile
├── .repository
├── .c7r
├── fs
│   └── etc
│       ├── group
│       ├── passwd
│       ├── profile
│       └── shadow
├── LICENSE.md
└── README.md
```

Usage
----------------------------------------------

### Build an image ###

Command: `c7r build <target-dir> [tag]`

Description: Build a docker image within the given directory (cached)

```
 $ c7r build ../dockerfiles-public/busybox/
 ____        _____     
 \ \ \    __|___  | __ 
  \ \ \  / __| / / '__|
  / / / | (__ / /| |   
 /_/_/   \___/_/ |_|   
                       
c7r » c[ontaine]r » stupid simple docker container build/push

 >> image-name: busybox:latest
 >> image-path: /home/.../dockerfiles-public/busybox
```

### Push an image to repository ###

Command: `c7r push <target-dir> [tag]`

Description: Push an existing image to the registry defined by target

```
 $ c7r push ../dockerfiles-public/busybox/
...
 >> image-name: busybox:latest
 >> image-path: /home/.../dockerfiles-public/busybox
  >> image retagged to https://myregistry.example.org/core/busybox:latest
The push refers to repository [myregistry.example.org/core/busybox]
329ef10ae799: Pushed 
831e37cf5494: Layer already exists 
latest: digest: sha256:ca18e7fdef865b6792b64cb0349c1d68f0656ba43f16371612652e5cefd03b01 size: 739
 >> image pushed to registry
```

### Reuild an image (nocache) ###

Command: `c7r rebuild <target-dir> [tag]`

Description: Build a docker image within the given directory (uncached)

```
 $ c7r rebuild ../dockerfiles-public/busybox/
...
 >> image-name: busybox:latest
 >> image-path: /home/.../dockerfiles-public/busybox
```


### Build+Push an image to repository ###

Command: `c7r update <target-dir> [tag]`

Description: rebuild an image (nocache) and push image to registry

```
 $ c7r update ../dockerfiles-public/busybox/
...
 >> image-name: busybox:latest
 >> image-path: /home/.../dockerfiles-public/busybox
  >> image retagged to https://myregistry.example.org/core/busybox:latest
The push refers to repository [myregistry.example.org/core/busybox]
329ef10ae799: Pushed 
831e37cf5494: Layer already exists 
latest: digest: sha256:ca18e7fdef865b6792b64cb0349c1d68f0656ba43f16371612652e5cefd03b01 size: 739
 >> image pushed to registry
```


License
----------------------------------------------

c7r is OpenSource and licensed under the Terms of [Mozilla Public License 2.0](https://opensource.org/licenses/MPL-2.0). You're welcome to [contribute](docs/CONTRIBUTING.md)