# ceg3120-cicd-Nejem-f26

- I cloned my github repo onto the local machine using git. this clreated a local copy of the repo that I can use to create and modify the project file, through VS code to edit the files and keep everything on track. 

## Creating the webiste Directory 

- I created a folder named web-content to organize the files for my website. it will contain the HTML pages and CSS stylesheet. thses files will later be copied in the docker container. 

### the about page 

- the about.html page provides additional information about the purpose of the website and the technology topics it covers. i made navigation links so visitors can move betweeen the homepage and the about page.

## Creating the Dockerfile 

- The Dockerfile tells Docker how to build an image containing your website and an apache web server. apache will serve the HTML and CSS files when someone accesses the containe. 

## Verifying Docker Installation 

- I checked that docker is installed, then I used docker info to verify that the doker engine was running. I ran docker run hello world as a test and it worked good! 

Command | Purpose | 

docker build: tells Docker to build an image.\
-t: gives the image a name and tag.\
tech-hub: our image name.\
latest: Identifies this as the latest build.\
.: uses the current directory as the build context, where docker find the dockerfile and website folder.

## Creating a Docker Hub Repository

- I created a public repository named tech-hub on Docker Hub. This repository will store the Docker image for my website so it can be accessed online. It will also be used later by GitHub Actions to automatically publish updated Docker images when changes are pushed to GitHub.

## Authenticating with Docker Hub

- I generated a Docker Hub Personal Access Token (PAT) with Read and Write permissions. I used this token to authenticate Docker from my local terminal with the docker login command. A personal access token provides a more secure way to authenticate without using my regular account password. The token was kept private and was not added to the project repository.

## tagging the Docker image 

- I used the docker tag command to assign my local Docker image a name that matches my Docker Hub username and repo. this allows Docker to identify which online repository should recceive the image when I pish it. 

## Pushing the docker image to docker hub 

- I used the docker push command to upload my tagged image to the public docker hub repo, it makes the image online so it can be downloaded, i verified the upload by visiting the docker hub repo. 

# Part 2 - github actions and docker hub

- the goal of part 2 is to automate what I did manually. instead of running docker build and docker push everytime i update the website. 

## Creating the Github actions workflow directory

- I created the .github/workflow directory in my repo. github actions uses this directory to locate workflow configuration files. I also created docker-publish.yml, which will contain the instructions for automatically building and publishing my Docker image.

## Creating the Docker Workflow 

- I created the github workflow. this workflow automaticlly builds and publishess my Docker image to Docker Hub whenever I push changes to the main branch. 
The workflow first checks out my repository, logs in to Docker Hub using the GitHub repository secrets, sets up Docker Buildx, and then builds and pushes the tech-hub image.

## Testing the Github Actions Workflow 

- I tested the Github actions workflow after adding it to my docker hub, the workflow works and it was completed successfully, shown by the green checkmark, and also verified that the docker image was published to docker also with the latest tag showing, This confirms that my CI/CD workflow can automatically build and publish my Docker image when changes are pushed to the main branch.

# Part 3 - Understanding Semantic Versioning 

