# Docker

-- How to docker --

1. docker-machine create --driver virtualbox Char
2. docker-machine ip Char
3. eval $(docker-machine env Char)
4. docker pull hello-world
5. docker run hello-world
6. docker run -d --name overlord -p 5000:80 --restart always nginx
7. docker inspect -f '{{ .NetworkSettings.IPAddress }}' overlord
8. docker run -it --rm alpine /bin/ash
9. #to start debian
   docker run -it --rm debian /bin/bash
   #to configure
   apt-get update
   apt-get upgrade
   apt-get install vim gcc git
10. docker volume create hatchery
11. docker volume ls
12. docker run -d --name spawning-pool -v hatchery:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Kerrigan -e MYSQL_DATABASE=zerglings --restart on-failure mysql --default-authentication-plugin=mysql_native_password
13. docker exec spawning-pool env
14. docker run -d --name lair -p 8080:80 --link spawning-pool:mysql wordpress http://192.168.99.100:8080/
15. docker run -d --name roach-warden -p 8081:80 --link spawning-pool:mysql phpmyadmin/phpmyadmin
16. docker logs -f 'machine_name'
17. docker ps
18. docker restart overlord
19. docker run -dt --name Abathur -v ~/Abathur:/root/app -p 3000:3000 python:2-slim
    docker exec Abathur pip install flask
    docker exec Abathur touch /root/app.py
    docker exec Abathur bash -c "echo 'from flask import Flask' >> /root/app.py"
    docker exec Abathur bash -c "echo 'app = Flask(**name**)' >> /root/app.py"
    docker exec Abathur bash -c "echo \"@app.route('/')\" >> /root/app.py"
    docker exec Abathur bash -c "echo 'def hello_world():' >> /root/app.py"
    docker exec Abathur bash -c "echo -e \"\treturn '<h1>Hello World</h1>' \" >> /root/app.py"
    docker exec Abathur bash -c "export FLASK_APP=/root/app.py && python -m flask run -h 0.0.0.0 -p 3000"
20. docker swarm init --advertise-addr $(docker-machine ip Char)
21. docker-machine create --driver virtualbox Aiur
