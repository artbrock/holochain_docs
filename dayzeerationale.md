# List of Dockerfiles

## Dockerfiles in `git clone https://github.com/metacurrency/holochain.git`

* `./Dockerfile`
  > Its useful to have this here in this list for completeness, and for people who want to guarantee they have the lastest master of holochain in their image, rather than relying on dockerhub having the latest version
  * creates an image from the latest master of github.com/metacurrency/holochain
* `./Dockerfile.coreDevelopment`
  > This is used for people who are developing the core code. Containers built on this image will allow the developer to manually interact with their new hc / bs servers from the command line
  * uses as a base image the result of `./Dockerfile`
  * splices the current `./ -r` over the top of the base image, and runs `make`, `make bs` and `make test`
  * will fail on build if `make test` fails

## Dockerfiles in `git clone https://github.com/<MY_PROJECT>/<MY_HOLOCHAIN_DNA>.git`
  > built from a skeleton which includes Dockerfiles
* `./Dockerfile.seedService` && `./Scripts/service.chain.seed`
  > This is used by the docker-compose.yml file to create an `hc seed <MY_HOLOCHAIN>` from the DNA in the local filesystem, which is shared between each `Dockerfile.serveInstance`
* `./Dockerfile.serveInstance` && `./Scripts/chain.joinAndServe`
  > This is used by the docker-compose.yml file to create an instance(s) of `hc server <MY_HOLOCHAIN>`
  * does `hc init <UNIQUE_CONTAINER_ID>`
  * does `hc join <MY_SEEDED_HOLOCHAIN>
  * exposes the port 3141 for the hc web server
  
# Overall rationale



[dayzee core dev](dayzeecoredev)