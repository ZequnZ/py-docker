# Python x Docker Handbook / Python x Docker 协作指南

[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)  [![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

In this repo, I will introduce how to use Docker to support Python project/application development
and provide some prototypes that can be used for your development.

Content：
- how to set up a **reproducible development environment** using Docker container for Python
   - install, build, run, execute
   - pull/push
- how to run your Python code interactively with a Docker container
   - volumn
   - --publish , -p  Publish a container's port(s) to the host
   - docker-compose
- how to dockerize an python application
  - 1 + 2
- The power of docker-compose
  - mocked service
## Install and how to run the container

1. Install [Docker](https://www.docker.com/)  
2. Clone this repository `git clone https://github.com/ZequnZ/py-docker.git`  
3. Build the Docker container `docker Build -t py-docker .`  
4. Run the container in the background `docker run --name py-docker -itd py-docker bash` and get the containerID from terminal.  
5. Execute the container `docker exec -it py-docker`  

I also use *Makefile* to make life easier:
Build the Docker container: `make build`  
Run the container in the background: `make run command=<command,default=bash>`  
Execute the container with command: `make exec command=<command,default=bash>`  

## Notes

### How to run jupyter notebook in the container

1. Bind a port when running the Docker container in the background and get the containerID
   `docker run -itd -p <hostPort>:<containerPort> zequnz/py-docker:<version>`
2. Execute the container `docker exec -it <containerID> bash`
3. Inside the contain launch the notebook assigning the port we just bind:
   `jupyter notebook --ip 0.0.0.0 --port <containerPort> --no-browser --allow-root`
4. Then open the jupyter notebook link on the browser.
   **Notice**
   It would be convenient to use the same _hostPort_ and _containerPort_, otherwise when open the jupyter notebook link, you may need to modify the port from _containerPort_ to _hostPort_.

## How to run docker volume for the container

1. Add the option when running the container
   `docker run -itd -p <hostPort>:<containerPort> -v $(pwd)/<VOLUME-FOLDER>:<CONTAINER-PATH> zequnz/py-docker:<version>`

   docker run -itd -p 8990:8990 -v $(pwd)/notebook:/app/notebook zequnz/py-docker:0.0.5
   jupyter notebook --ip 0.0.0.0 --port 8990 --no-browser --allow-root

## TODO

- [x] Understand how to save jupyter notebooks from docker container to local machine: **Docker volume**  
- [x] Add a [Makefile](./Makefile) to simplify the running commands.  
- [x] Add more important / useful python packages to requirements.txt  
- [ ] Docker-compose
- [ ] Think & learn how to add a selection menu when running the docker container  
