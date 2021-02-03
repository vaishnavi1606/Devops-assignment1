Downloaded the code of nodejs using this url: https://drive.google.com/file/d/1wVJ1_zVc_-3foDVodHdIS-pvvftjFLU4/view ?usp=sharing 
Installing docker on system, restart and enabling the service of docker.
Installing docker compose using this command
   sudo curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
   docker-compose --version
created a dockerfile using this code for nodejs application. 
