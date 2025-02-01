# devcontainer

## Overview

This repo documents usage of devcontainer for development. For setup basic usage, refer to the official document [vscode/devcontainers](https://code.visualstudio.com/docs/devcontainers/containers)

## Example Custom Docker Compose (Python3.6 + MySQL)

> This example uses the custom devcon image build using the following [instruction](python-3/README.md).

- Create the folder .devcontainer in the workspace root directory, the folder stucture shown below
```
workspace/
└── .devcontainer/
    ├── devcontainer.json
    ├── docker-compose.yml
    └── Dockerfile.python
```

- Add `devcontainer.json`. Modify customizations accordingly.
```json
{
	"name": "Python 3 & MySQL",
	"dockerComposeFile": "docker-compose.yml",
	"service": "app",
	"workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",

	// Features to add to the dev container. More info: https://containers.dev/features.
	// "features": {},

	// Use 'forwardPorts' to make a list of ports inside the container available locally.
	// This can be used to network with other containers or the host.
	// "forwardPorts": [5432]

	// Use 'postCreateCommand' to run commands after the container is created.
	// "postCreateCommand": "pip install --user -r requirements.txt",

	// Configure tool-specific properties.
	// "customizations": {},

	// Uncomment to connect as root instead. More info: https://aka.ms/dev-containers-non-root.
	// "remoteUser": "root"
}
```

- Add `Dockerfile.python`
```Dockerfile
FROM devcon-python3.6:latest

ENV PYTHONUNBUFFERED 1

# [Optional] If your requirements rarely change, uncomment this section to add them to the image.
# COPY requirement.txt /tmp/pip-tmp/
# RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirement.txt \
#    && rm -rf /tmp/pip-tmp

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>
```


- Add `docker-compose.yml`
```yml
version: '3.8'

services:
  app:
    build:
      context: ..
      dockerfile: .devcontainer/Dockerfile.python

    volumes:
      - ../..:/workspaces:cached

    # Overrides default command so things don't shut down after the process ends.
    command: sleep infinity

    # Runs app on the same network as the database container, allows "forwardPorts" in devcontainer.json function.
    network_mode: service:db

    # Use "forwardPorts" in **devcontainer.json** to forward an app port locally.
    # (Adding the "ports" property to this file will not forward from a Codespace.)

  db:
    image: mysql:8.0.33
    restart: unless-stopped
    volumes:
      - mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: 1234

volumes:
  mysql-data:
```