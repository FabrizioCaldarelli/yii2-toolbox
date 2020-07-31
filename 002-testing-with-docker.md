# 002 - Testing with Docker

## Structure

This doc will set docker as first subfolder of document root. This will allow to handle all files, also that are not included in app sources.

**docroot** is an alias of DOCUMENT_ROOT.

## Installation

We assume that Yii2 sources are in **docroot**/**src** folder. <u>Adjust path as you needed</u>.

1 - Copy `data/001-new-project/docker` folder content to **docroot** folder.

```sh
cp -aRv data/001-new-project/docker docroot
```

2 - Change dir to docroot/docker

```sh
cd docroot/docker
```

3 - Build docker image

```sh
docker-compose build
```

4 - Start docker container

```sh
docker-compose up -d
```

5 - Enter in docker container

```sh
docker-compose exec web bash
```

6 (optional) - Init yii2 and apply migrations (if needed)

Pointing to yii2 source folder, launch these commands:

```sh
php /app/src/init --env=Development --overwrite=All 
php /app/src/yii migrate --interactive=0
```

The installation is finished!

## Checks

### Frontend and backend

Backend is at: `http://localhost:8000/src/backend/web`
<br />
Frontend is at: `http://localhost:8000/src/frontend/web`

### PhpMyAdmin

PhpMyAdmin is at: `http://localhost:8888`

- host: mysql
- username: dev
- password: dev
- database: dev

### X-Debug

Also X-Debug is present in this docker configuration.

To debug with VSCode, starting from `docroot` folder, create a `.vscode` folder and put this content into `launch.json` file (complete path: **docroot/.vscode/launch.json**):

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9005,
            "pathMappings": {
                "/app" : "${workspaceFolder}"
            },
            "xdebugSettings": {
                "max_children": 200,
                "max_data": 512,
                "max_depth": 4,
                "show_hidden": 1
            }            
        },
        {
            "name": "Launch currently open script",
            "type": "php",
            "request": "launch",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "port": 9005,
            "pathMappings": {
                "/app": "${workspaceFolder}"
            },
            "xdebugSettings": {
                "max_children": 200,
                "max_data": 512,
                "max_depth": 4,
                "show_hidden": 1
            }            
        }
    ]
}
```

**Attention:** the docker container listens to 9005 port
