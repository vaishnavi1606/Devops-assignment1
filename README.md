1.	Downloaded the code of nodejs using this url: https://drive.google.com/file/d/1wVJ1_zVc_-3foDVodHdIS-pvvftjFLU4/view ?usp=sharing 

2.	Installing docker on system, restart and enabling the service of docker.
$  yum install docker –y
 $ systemctl restart docker.service
 $ systemctl enable docker.service

3.	Installing docker compose using this command
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
              $ sudo chmod +x /usr/local/bin/docker-compose
              $ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
              $ docker-compose --version
4.	Created a Dockerfile using this code for nodejs application.
   FROM node:latest
   ENV NODE_OPTIONS --max-old-space-size=2048
   WORKDIR /app
   COPY ./package* ./
   COPY ./* ./
   RUN ls -al
   RUN npm install && \
       npm cache clean --force
  COPY . .
  EXPOSE 51005
  CMD ["node", "index.js"]

NOTE: 
•	In this Docker file I used Fundamental instruction, Configuration instruction and Execution instruction which is as follows
•	Docker file is stack of multiple layers created from dockerfile instruction 
 
a.	Fundamental Instrution
1.	FROM:  It specifying the base image of the resultant docker image
2.	ENV NODE_OPTIONS:  It specifying size of memory,cpu limit,etc.
b.	Configuration Instruction
1.	RUN: Which is use for RUN the command on the top of the image
2.	COPY: is used for coping some file from host to container
3.	WORK DIR: using working directory of container

c.	Execution Instruction
1.	EXPOSE: is used for port listing
2.	CMD: is used for container executable.


5.	Created docker-compose.yml file for mysql and mango DB

              version: "3"
                services:
                    web:
                         container_name: backend_demo
                         build: .
                         restart: always
                         ports:
                         - "51005:51005"
                         links:
                           - mongo
                           - mysql
                        mongo:
                        container_name: mongo
                        image: mongo
                        volumes:
                             - ./data:/data/db
                        restart: on-failure    
                        ports:
                          - "27017:27017"
                       mysql:
                              image: mysql:5.7.28
                       volumes:
                             - ./data:/data/db
                       environment:
                        MYSQL_ROOT_PASSWORD: 'root'
                        MYSQL_DATABASE: 'backend_demo'
                        restart: on-failure
                        ports: 
                        - "3306:3306"

        
  NOTE: 
•	 In this docker-compose.yml file each service are running separately.

•	3 different containers are created and having 3 separate images as per the the docker-compose.yml file

•	In this docker-compose.yml file docker is instructed  to restart the container if it fails. 

•	Adding mango db and container into that and mapping port of the container

•	Adding mysql db container into that mapping port of the container

•	And link of the both container into nodejs application container it will reachable to the node application container

•	persistent storage is used in the docker compose for sync for database in host to the docker container. This is user for mounting volume in it

•	Persistent storage it will help of backup of the database of the container. 

6.	Execute the command 
$ docker-compose up
 
Then separate container will be create they will be link together, node application and the database.

7.	Install nginx.
a.	Install nginx using this command
1.	$ yum install nginx  -y.
2.	Restart the service of nginx
$ systemctl restart nginx
$ systemctl enable nginx
3.	Create a directory site-avaliable
$  mkdir /etc/nginx/sites-available
$  mkdir /etc/nginx/sites-enabled
4.	Create a link between them 
$  ln –s /etc/nginx/sites-available /etc/nginx/sites-enabled

8.	Create self signed certificate for SSL using the following command

1.	 mkdir /etc/ssl/private
2.	 chmod 700 /etc/ssl/private
3.	  sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
4.	 sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048

9.	Add config file for redirection of http to https
1.	Add app.conf file into it

2.	$ /etc/nginx/sites-available/app.conf


 server {
    listen 80;

    server_name  172.31.29.33:51005;
    return 301 https://172.31.29.33:51005;

 }
 
3.	Add ssl.conf

server {
    listen 443 http2 ssl;


    server_name  172.31.29.33:51005;

    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
    ssl_dhparam /etc/ssl/certs/dhparam.pem;
}






 
