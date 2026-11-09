# ceg3120-cicd-Nejem-f26

- I cloned my github repo onto the local machine using git. this clreated a local copy of the repo that I can use to create and modify the project file, through VS code to edit the files and keep everything on track. 

## Creating the webiste Directory 

- I created a folder named web-content to organize the files for my website. it will contain the HTML pages and CSS stylesheet. thses files will later be copied in the docker container. 

### the about page 

- the about.html page provides additional information about the purpose of the website and the technology topics it covers. i made navigation links so visitors can move betweeen the homepage and the about page.

## Creating the Dockerfile 

The Dockerfile tells Docker how to build an image containing your website and an apache web server. apache will serve the HTML and CSS files when someone accesses the containe. 