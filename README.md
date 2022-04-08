# singularityhub EMBL Rome


1. [Introduction](#introduction) 
2. [Pulling](#pulling) 


## Introduction

This repository aims at sharing singularity images among the EMBL community. We try to follow a strict model to provide uniformly designed singularities. Please let us know if we should modify anything.

## Pulling

**Please read the entire section before trying to pull any singularities**.

To pull an existing singularity, first have a look at the image of interest in the list [here](https://git.embl.de/descoste/singularityhub-emblrome/container_registry) or in this [folder](https://git.embl.de/descoste/singularityhub-emblrome/-/tree/main/recipes). 

Copy the script below in a `download.sh` file and run the command: `bash dowload.sh username containername imagename`. For example, `bash download.sh descoste fastqcv0019.sif 'fastqc:0119'`.

```
#!/usr/bin/bash

USERNAME=$1
CONTAINERNAME=$2
IMAGE=$3

singularity pull --docker-username $USERNAME --docker-password $SINGULARITY_DOCKER_PASSWORD $CONTAINERNAME oras://git.embl.de:4567/descoste/singularityhub-emblrome/$IMAGE
```

**Important**: You need to define a git token to be able to use the `$SINGULARITY_DOCKER_PASSWORD` variable. Follow these steps:

1) Click on your avatar at the top right of your gitlab page.
2) Click on `preferences`.
3) Click on `Access Tokens`.
4) Enter a Token name. ex: "singularitypull".
5) In the `Select scopes` section, select `read_registry`.
6) Click `Create personal access token`.
7) At the beginning of the new loaded page, click on the folder icon to copy your new personal access token.
8) Edit your `.bashrc` (`emacs -nw ~/.bashrc` or `vim ~/.bashrc`) by adding `export SINGULARITY_DOCKER_PASSWORD="paste_your_copied_access_token_here"` wherever you like.
9) After closing your editor, run `exec bash`.
10) Now try to pull a particular singularity following the instructions above.

