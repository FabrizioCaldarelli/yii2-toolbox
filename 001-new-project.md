# 001 - Start a new Project

## Structure

I use a structure that isolates app sources from other external files.

The structure is:
* DOCUMENT_ROOT (aka docroot)
    * public_files
    * some_other_public_folder
    * **webapp**
        * **src**

**webapp** folder is synced with git

So create a DOCUMENT_ROOT (docroot) folder and inside it create **webapp**.



## Installation

These are steps to start a new Yii2 project, using Advanced Template.

1 - Create DOCUMENT_ROOT (docroot) and **webapp** subfolder

```sh
mkdir docroot
mkdir docroot/webapp
```

2 - Change dir to docroot/webapp

```sh
cd docroot/webapp
```

3 - Download composer

```sh
curl -sS https://getcomposer.org/installer | php
```

4 - Create advanced template project inside **src** subfolder

```sh
php composer.phar create-project --prefer-dist yiisoft/yii2-app-advanced src
```

5 - Copy `data/001-new-project/docker` folder content to **docroot** folder. So copied content will be at same level of **webapp** folder.

```sh
cp -aRv data/001-new-project/docker docroot
```

6 - Change dir to docroot/docker

```sh
cd ../docker
```

7 - Build docker image

```sh
docker-compose build
```

8 - Start docker container

```sh
docker-compose up -d
```

9 - Enter in docker container

```sh
docker-compose exec web bash
```

10 - Configure environments/dev/common/config/main-local.php file, setting db component:

```php
<?php
return [
    'components' => [
        'db' => [
            'class' => 'yii\db\Connection',
            'dsn' => 'mysql:host=mysql;dbname=dev',
            'username' => 'dev',
            'password' => 'dev',
            'charset' => 'utf8mb4',
        ],
        'mailer' => [
            'class' => 'yii\swiftmailer\Mailer',
            'viewPath' => '@common/mail',
            // send all mails to a file by default. You have to set
            // 'useFileTransport' to false and configure a transport
            // for the mailer to send real emails.
            'useFileTransport' => true,
        ],
    ],
];
```

11 - Init yii2 and apply migrations

```sh
php /app/webapp/src/init --env=Development --overwrite=All 
php /app/webapp/src/yii migrate --interactive=0
```

12 (optional) - Remove composer.phar file

```sh
rm docroot/webapp/composer.phar
```

The installation is finished!

## Checks

### Frontend and backend

Backend is at: `http://localhost:8000/webapp/src/backend/web`
<br />
Frontend is at: `http://localhost:8000/webapp/src/frontend/web`

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
            }
        }
    ]
}
```

**Attention:** the docker container listens to 9005 port