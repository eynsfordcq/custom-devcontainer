# python3-devcontainer

## Overview

There are some versions where devcontainer image is not officially available. Follow the steps to build your own.

## Build 

1. Copy `base.Dockerfile` and name it accordingly, e.g. `Dockerfile.python3.6`

2. Modify the first two lines to the image you want 
```Dockerfile
# ARG VARIANT=3-bullseye # comment out this line
FROM python:3.6-buster # modify this line to the version you want 
```

3. Build the image 
```bash
# in .devcontainer folder 
# put the tag you want, for consistency, use devcon-<variant>:latest
# add registry path in the tag if you wish to upload to gitlab container registry
docker build -t devcon-python3.6 -f Dockerfile.python3.6 .
```

4. (Optional) Push to Registry
```bash
# make sure you have already login to the registry
docker push <registry>/devcon-python3.6:latest
```

## Reference 
[github/vscode-dev-containers](https://github.com/microsoft/vscode-dev-containers/tree/main/containers/python-3)