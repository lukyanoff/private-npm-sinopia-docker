# Private Sinopia docker container

This repo shows how to start a private Sinopia to store private scoped packages


## Install 

Install docker and docker-compose. Then get into this repo folder and run: 

```sh
$> docker-compose up sinopia
```

## Usage 

This container registry can be used to store private packages with a minimal setup. 


### Global configuration

If you want to store all your private packages for scope `@yourcompany/*` you need to add the registry to your global `$HOME/.npmrc` file.

```sh 
$> npm config set @yourcompany:registry http://localhost:4873
```

After that, every package you install/publish under `@yourcomany` namespace will be directed to that registry instead of public NPM.


### Per project configuration

If you deploy your project with any dependency from your private registry, you may also need a `.npmrc` shipped with your project files to have access to this registry.

Add this line to your project's `.npmrc`: 

```ini
@test:registry=http://localhost:4873/
```


## Users

### Adding a user

You can add users to the registry with

```sh
$> npm adduser --registry http://localhost:4873/
```

### Login with a user

After you added a user you can login to the registry and publish packages with: 

```sh
$> npm login --registry http://localhost:4873/ --scope @yourcomany
```

## Aliasing

Every `npm` command that you need to direct to this registry must be prefixed with `--registry http://localhost:4873/`, so you could ease the pain with an alias

```sh
$> alias npm-myco="npm --registry http://localhost:3873/" 
```

Then use it like this: 
```sh
$> cd my-very-secret-private-package
$> npm-myco publish // and your package will be publish to your private registry
```

## Backups 

The image stores packages in `/opt/sinopia/storage` forlder that is shared with the host. Check the provided `docker-compose.yml` and change the volume to your needs.



