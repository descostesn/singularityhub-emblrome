# singularityhub EMBL Rome


1. [Introduction](#introduction) 
2. [Pulling](#pulling) 
3. [Contributing](#contributing)


## Introduction

This repository aims at sharing singularity images among the EMBL community. We try to follow a strict model to provide uniformly designed singularities. Please let us know if we should modify anything.

**Important: Since we do not have a premium account, I tried to find some workaround. We cannot secure completely that master will stay green. Please be careful to strictly follow the instructions.**


## Pulling

**Please read the entire section before trying to pull any singularities**.

To pull an existing singularity, first have a look at the image of interest in the list [here](https://git.embl.de/descoste/singularityhub-emblrome/container_registry) or in this [folder](https://git.embl.de/descoste/singularityhub-emblrome/-/tree/main/recipes). 

Copy the script below in a `download.sh` file and run the command: `bash dowload.sh username containername imagename`. For example, `bash download.sh descoste fastqcv0019cv8.sif 'fastqc:0119cv8'`.

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

If it does not work please do:

1) Add the remote: `singularity remote add --no-login embl https://git.embl.de:4567`
2) Use the remote: `singularity remote use embl`

## Contributing

This repository is maintained by Nicolas Descostes.

**Important: Since we do not have a premium account, I tried to find some workaround. We cannot secure completely that master will stay green. Please be careful to strictly follow the instructions.**

**Important Bis: Each singularity should contain a single tool. Contact me ahead if you plan otherwise.**


To add a new singularity recipe, you need to:

1) Clone the repository: `git clone git@git.embl.de:descoste/singularityhub-emblrome.git`
2) Enter the folder: `cd singularityhub-emblrome/`
3) Position yourself on the "submission" branch: `git checkout submission`.
4) Make sure that the content of the branch is up-to-date: `git reset --hard main`
5) Add a singularity recipe inside `recipes` in the adapted folder. 

**Respect the naming format `Singularity.toolName-tag` (with a upper-case S). Please use common sense to choose the folder**. If you are not sure, please contact me by email or by chat.

For instance, if you want to submit fastqc version 0119cv8, your file name will be `Singularity.fastqc-0119cv8`.

6) Commit and push to the repository: `git add myrecipe && git commit -m "initial commit" && git push origin submission"
7) Modify `.gitlab-ci.yml` in the "submission area" using the following template (replace `toolName`, `tag`, and `path_to_recipe_folder`):

```
toolName-tag-test:
  extends: .templateTest
  variables:
    BASENAME: toolName
    TAG: tag
    RECIPE_PATH: path_to_recipe_folder
```

For instance, if you want to submit fastqc version 0119cv8, your rule name will be `fastqc-0119cv8-test` and the path to the recipe `/g/romebioinfo/tmp/singularityhub-emblrome/recipes/quality-control/fastqc`.


8) In the following instruction, **please add toolName-tag-test` as a commit message**.
9) Push the file `.gitlab-ci.yml` to the repository: `git add .gitlab-ci.yml && git commit -m "toolName-tag-test" && git push origin submission`.
10) Visit this [page](https://git.embl.de/descoste/singularityhub-emblrome/-/merge_requests) to submit a merge request.

- As title: toolName-tag-test
- description: A one-line sentence to explain what the tool is. Please precise any important information as well.
- Reviewer: Choose Nicolas Descostes
- **Be careful:** Uncheck the `Delete source branch when merge request is accepted.` before submitting.

11) Now it is time to test the build of your singularity. You will see a gear on the right of `Detached merge request pipeline #32160 waiting for manual action for `. Click on it and hit the play button next to your rule.
12) In the `CI/CD > jobs` (menu on the left), you can see your job running.
13) Once your job passes the test (green checkmark), I will merge and deploy your singularity. I will let you know when this is done.


