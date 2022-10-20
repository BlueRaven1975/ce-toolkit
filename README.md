# CLOUD ENGINEERING TOOLKIT

This is a collection of tools to perform the typical operations involved in
Cloud Engineering work.

## How to build

1. Install Docker on your machine (https://docs.docker.com/get-docker/)

2. Open your favorite shell - command prompt, PowerShell, bash, etc. - and cd to
the directory you cloned this repository into (the one containing this README.md
file)

3. Create a Docker volume in which you'll store all your data (e.g. code repos),
so that they persist if you have to rebuild the toolkit: `docker volume create ce-toolkit-data`

4. Set your desired username in an environment variable:

* `export USER=<username>` (for Bash and Bash-compatible shells)
* `$env:USER = '<username>'` (for PowerShell)

5. [OPTIONAL] If you want to pin specific versions of the tools (currently supported
for Ansible and Terraform only), set the corresponding environment variables:

* `export ANSIBLE_VERSION=x.y.z`
* `export TERRAFORM_VERSION=x.y.z` (for Bash and Bash-compatible shells)

* `$env:ANSIBLE_VERSION = 'x.y.z'`
* `$env:TERRAFORM_VERSION = 'x.y.z'` (for PowerShell)

6. Launch the build process: `docker-compose build`

7. Once the build is finished, run your brand new CE Toolkit container:

* `docker run -it --name ce-toolkit --hostname ce-toolkit --network=host
--env GIT_COMMITTER_NAME="<yourfullname>" --env GIT_COMMITTER_EMAIL="<youremail>"
--env GIT_AUTHOR_NAME="<yourfullname>" --env GIT_AUTHOR_EMAIL="<youremail>"
-v ce-toolkit-data:/home/$USER/data --add-host=ce-toolkit:127.0.0.1 ce-toolkit`  
(for Bash and Bash-compatible shells)

* `docker run -it --name ce-toolkit --hostname ce-toolkit --network=host
--env GIT_COMMITTER_NAME="<yourfullname>" --env GIT_COMMITTER_EMAIL="<youremail>"
--env GIT_AUTHOR_NAME="<yourfullname>" --env GIT_AUTHOR_EMAIL="<youremail>"
-v ce-toolkit-data:/home/$env:USER/data --add-host=ce-toolkit:127.0.0.1 ce-toolkit`  
(for PowerShell)

> **NOTE**: replace *\<yourfullname\>* and *\<youremail\>* with your own data,
e.g. "Luther Blissett" and "luther.blissett@internet.com" respectively; these
are shown in your commits should you make one directly from inside the toolkit
instead of using your local IDE

8. When you exit the shell, the container will also exit; if you don't want to
always run a new one, you can also restart it manually and connect back to it
with the following command: `docker exec -it ce-toolkit bash`.  
This is particularly useful if you use some form of integration such as
[Remote - Containers for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
(avoids you to reinstall vscode-server, extensions, etc.).
