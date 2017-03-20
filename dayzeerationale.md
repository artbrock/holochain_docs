# List of Dockerfiles

> the `./` context is assumed to be the root of a 
> git clone https://github.com/metacurrency/holochain.git`

* `./Dockerfile`
  > Its useful to have this here in this list for completeness, and for people who want to guarantee they have the lastest master of holochain in their image, rather than relying on dockerhub having the latest version
  * creates an image from the latest master of github.com/metacurrency/holochain
* `./Dockerfile.coreDevelopment`
  > This is used for people who are developing the core code. Containers built on this image will allow the developer to manually interact with their new hc / bs servers from the command line
  * uses as a base image the result of `./Dockerfile`
  * copies the current `./ -r` over the top of the base image, and runs `make`, `make bs` and `make test`
  * will fail on build if `make test` fails



# Overall rationale



[dayzee core dev](dayzeecoredev)