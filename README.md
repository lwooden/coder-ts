## Custom Coder Image Workflow
The way to go is to create custom images of coder with a filesystem mirror of everything I need (providers and modules)

Test Harness
MAC OS Testing
- use Dockerfile_MacOS to validate that everything works
- run docker run --rm --entrypoint "" -it <IMAGEID> /bin/sh to overwrite entrypoint and gain access to the shell
- create a main.tf in coder and copy the contents of test.tf and run terraform init to confirm that everything works
- my test.tf is based off of the DOCKER coder template; I will need to do this for other templates that I want to use

AMD64 Testing (The Real Test)
- use Dockerfile to pull in amd64 versions of all providers
- follow testing steps (above)
- once successful, build platform specfic image by running docker build -t low-coder:v1.0_ts_fsmirror . --no-cache --platform linux/amd64

Package up the Image
docker save <IMAGE> | gzip > IMAGE_NAME.tar.gz

## Redhat Ubi 9 Based Workspace Workflow

Build using Dockerfile_ubiWorkspace
docker build -t docker.io/lwooden/ubi_coderbase:9.MINOR . --platform linux/amd64 -f Dockerfile_ubiWorkspace --no-cache --label "description=pin to 9.7, bring back java25 and wrf"

Override the Entry Point to Test functionality Locally
docker run --rm -it --entrypoint "" <IMAGE> /bin/bash