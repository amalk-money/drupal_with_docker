# Drupal on docker
## Overview of the project
I’m implementing drupal web applicaton using docker container integrated with MYSQL and POSTGRES database. This project is done on Red Hat Linux and so all the commands are with reference to this OS.
## Setting up the docker
I’m using docker community edition on the Red Hat Linux Enterprise Edition. To run any OS or application, we require its image to work on it. Using the command ‘’’bash docker pull image-name’’’ we can get the image.
## Pulling the required Images:
### DRUPAL Image:
<ul><li>I used “drupal:8-apache“ version to carry out this project. </li>
<li>Command to pull this image: ‘’’bash docker pull drupal:8-apache’’’ </li>
<li>For more information about DRUPAL image, go to : </li></ul>
### MySQL Image
<ul><li>I used “mysql:5.7”  version to carry out this project. </li>
<li>Command to pull this image: ‘’’bash docker pull mysql:5.7 ’’’ </li>
<li>For more information about MYSQL image, go to : </li></ul>
### POSTGRES Image
<ul><li>I used “postgres:10” version to carry out this project. </li>
<li>Command to pull this image: “docker pull postgres:10’’’ </li>
<li>For more information about POSTGRES image, go to : </li></ul>
## How to use DRUPAL Image
<ul><li> ‘’’bash #docker run -dit  - - name drupal-name  -p  8088:80 drupal:8-apache ‘’’ will run the basic drupal web application.</li>
<li>And to access it, use http://localhost:8088 or http://host-ip:8088 to access the application on the web browser. </li>
<li>This image supports multiple databases, so I thought to integrate it with MYSQL and POSTGRES database. </li></ul>
## How to use MYSQL Image
<ul><li>To integrate Drupal with MYSQL, first we need to run the MYSQL container and configure with the required environment variables:</li>
‘’’bash #docker run - dit - -name  drupalDB  - v mysql_storage:/var/lib/mysql/  - -network drupalNET -e MYSQL_ROOT_PASSWORD=rootpass -e MYSQL_USER=user -e MYSQL_PASSWORD=passcode -e MYSQL_DATABASE=drupal  mysql:5.7 ‘’’ 
<li>New user called “user” is created with password “passcode” and created a database called “drupal”.</li></ul>
## How to use POSTGRES Image:
<ul><li>Similarly like MYSQL, we can configure the POSTGRES image in the same way, just differs in the environment variable name:</li>
‘’’bash #docker run - dit - -name  drupalPOST  - v post_storage:/var/lib/mysql/  - -network drupalNET
-e POSTGRES_USER=user
-e POSTGRES _PASSWORD=passcode
-e POSTGRES _DB=drupal 
postgres:10 ‘’’
<li>A new user called “user” is created with password “passcode” and created a database called “drupal”.</li></ul>
## Integrating Drupal with database:
### MYSQL
While running the drupal container, we need to give the mysql username and password and using link attribute to link to the database.
“#docker run -dit  - - name drupal-name  -p  8088:80
-e MYSQL_USER=user
-e MYSQL_PASSWORD=passcode
- - link drupalDB drupal:8-apache”
### POSTGRES
Similarly, for the drupal to integrate with postgress, while running the container we need to give username and password and link attribute to link to database.
“#docker run -dit  -name drupal-name  -p  8088:80
-e POSTGRES_USER=user
-e POSTGRES _PASSWORD=passcode
- - link drupalDB drupal:8-apache”
## Volumes associated with drupal:
•	Drupal Image requires volume to store the data. It uses volume to store the site, module, theme and profile data.
“docker run - dit  - - name drupalAPP  - - network  drupalNET 
	-v drupal-module:/var/www/html/modules
	-v drupal-sites:/var/www/html/ sites
	-v drupal- themes:/var/www/html/themes
-v drupal- profiles:/var/www/html/profiles
drupal:8-apache”
## Docker-compose:
•	To start with docker compose, we need to install the docker compose first because it doesn’t come with the docker software itself.
•	So here I created a docker-compose file for the above setup in a yaml format.
•	All you need to do is to get the docker-compose.yml file and run the command:                “#docker-compose up”.
 
•	This will provide you the infrastructure into your machine, with all the volumes, network automatically created when the program runs.
## Getting started with the drupal application:
•	To run the drupal application, go to your browser and type “localhost:8080”
 
•	To access the application using ip address, use the host ip to get it.
 
## Setting up the drupal application
STEP 1: Choose your preferred language and save it.
STEP 2: Select an installation profile of your choice.
STEP 3: Setup the database.
a. Choose the database type.
b. Enter the database name , in my compose file I created “drupal” as database.
c. Enter the username, in my compose file I created “user” as username.
d. Enter the password, in my compose file I created “passcode” as password.
e. In advanced option, Enter the host name as:
	If selected MYSQL database, enter “drupalDB”.
	If  selected POSTGRES database, enter “drupalPOST”.
And save it.
STEP 4: After installation, configure your site as you desire by giving the site name, email address, username, password and save and continue.
Your account will be created and now you are ready to create your first content.
 
## Stopping the compose:
•	To stop docker compose, we use the command “#docker-compose down”
 

## Troubleshooting:
•	In the Drupal application, if having error with connecting to the database, try deleting all the volumes using the command “#docker volume rm -  f $(docker volume ls)” and stop the compose and once again run the docker compose file.
•	If some connection error is encountered, try disabling the firewall.
•	If some unexpected error encounters, try disabling SELinux and try it. If still remains, try removing all the containers and again running the compose file.

## For improvements and updation, fell free to correct me.
