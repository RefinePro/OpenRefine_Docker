# Docker OpenRefine distribution

Custom docker image to run [OpenRefine](https://openrefine.org/) v3.4.1. This configuration includes the following:

* `Dockerfile`: build image `docker-openrefine:1.0` (Needs access to DockerHub and https://github.com/OpenRefine/OpenRefine/releases/download)
* `docker-compose.yml`: start, stop and easy configuration for docker image `docker-openrefine:1.0`
* `or-configuration`: folder with OpenRefine default configuration and the Workspace File Extension


## Running with docker

* Build the container: `docker build . -t docker-openrefine:1.0`
* Run the container: `docker run --network host docker-openrefine:1.0`
    * You can define a volume to persist the OpenRefine Workspace with: `docker run --network host -v /home/USER/docker-openrefine/data:/or/data docker-openrefine:1.0`
    * For Mac and Windows network mode `--network host` is not supported. Run with `docker run -p 3333:3333 docker-openrefine:1.0`
* Open http://127.0.0.1:3333 in your browser
* Stop the container with `ctrl + c` in the terminal that is running `docker run...`

### Enable/Disable create project options

During the `docker build ...` command, you can set the following arguments to disable any menus from the `Create Project` page. This option lets the system administrators control how users can import data into the OpenRefine instance. For example, you can limit import to only what is available on the OpenRefine instance using `Workspace Data`)

By default, all options are enabled. 

* **This Computer:** `docker build --build-arg THIS_COMPUTER=false`
* **Web Addresses (URLs):** `docker build --build-arg WEB_ADDRESSES=false`
* **Clipboard:** `docker build --build-arg CLIPBOARD=false`
* **Database:** `docker build --build-arg DATABASE=false`
* **Workspace Data:** `docker build --build-arg WORKSPACE=false`
* **Google Data:** `docker build --build-arg GOOGLE=false`

**Example command to disable the Clipboard and the Workspace Data options** 
* `docker build --build-arg CLIPBOARD=false --build-arg WORKSPACE=false . -t docker-openrefine:1.0`

## Running with docker-compose

* Build the container: `docker build . -t docker-openrefine:1.0`
* Start the container: `docker-compose up -d`
* Open http://127.0.0.1:3333 in your browser  
* Stop the container: `docker-compose stop`

**Note:** additional configuration can be added/updated in `docker-compose.yml`

## OpenRefine Memory allocation

* Update values in `refine.ini` for variables `REFINE_MEMORY` and `REFINE_MIN_MEMORY` 
* Re-create the container `docker build . -t docker-openrefine:1.0`

## Extension details and configuration

### Workspace File extension

#### Source Code: 

* [RefinePro/OpenRefine_local_file_extension](https://github.com/RefinePro/OpenRefine_local_file_extension)

#### Version compiled in this docker image:

* Compilation date: 2021-01-04
* Compiled by: RefinePro
* Version 1.0.1 

#### Update Docker

When new development are made on the public repository:

* Pull the changes and resolve potential conflicts introduced by the new version.
* Compile and update the docker image with the compiled files in `or-configuration/extensions/local-file-system`
* Update README.md with new version details

#### Documentation: 

Location to load the files is configured with the environment variable `EXT_LOCAL_FILE_SYSTEM` in the `Dockerfile` archive and will load all files from folder `workspace-files`

* If you are running OpenRefine with `docker run...` adding/removing files in folder `workspace-files` requires a new build with `docker build . -t docker-openrefine:1.0` to see the updates in OpenRefine.
* If you are running OpenRefine with `docker-compose ` adding/removing files in folder `workspace-files` will be reflected immediately as soon you refresh the page to create new projects 

## Acknowledgements

* The seed Dockerfile came from https://github.com/opencultureconsulting/openrefine-docker/blob/master/3.3/Dockerfile
