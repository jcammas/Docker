# Docker

-- How to docker --

1. docker-machine create --driver virtualbox Char
2. docker-machine ip Char
3. eval $(docker-machine env Char)
4. docker pull hello-world
5. docker run hello-world

   https://medium.com/crowdbotics/a-complete-one-by-one-guide-to-install-docker-on-your-mac-os-using-homebrew-e818eb4cfc3
   follow this for 1 to 5

6. docker run -d --name overlord -p 5000:80 --restart always nginx
   run -d = tache de fond --name overlord = nom du container -p = port 5000:80 bind

7. docker inspect -f '{{ .NetworkSettings.IPAddress }}' overlord

   https://webdevdesigner.com/q/how-to-get-a-docker-container-s-ip-address-from-the-host-2973/

8. docker run -it --rm alpine /bin/ash

   https://qastack.fr/programming/35689628/starting-a-shell-in-the-docker-alpine-container

9. #to start debian
   docker run -it --rm debian /bin/bash
   #to configure
   apt-get update
   apt-get upgrade
   apt-get install vim gcc git
   
   https://doc.ubuntu-fr.org/apt-get

10. docker volume create hatchery

    https://docs.docker.com/engine/reference/commandline/volume_create/

11. docker volume ls

    https://docs.docker.com/engine/reference/commandline/volume_ls/

12. docker run -d --name spawning-pool -v hatchery:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Kerrigan -e MYSQL_DATABASE=zerglings --restart on-failure mysql --default-authentication-plugin=mysql_native_password
    -v = stocker dans le volume

13. docker exec spawning-pool env

    https://dev.wakonda.guru/article/read/118/docker-afficher-toutes-les-variables-d-environnement-d-un-container.docker-afficher-toutes-les-variables-d-environnement-d-un-container

14. docker run -d --name lair -p 8080:80 --link spawning-pool:mysql wordpress 

    http://192.168.99.100:8080/
    https://mathisthuault.wordpress.com/2018/10/14/dockeradministration-partie-2-les-conteneurs-docker-les-dockerfiles/#:~:text=2.1.4)

15. docker run -d --name roach-warden -p 8081:80 --link spawning-pool:mysql phpmyadmin/phpmyadmin
16. docker logs -f 'machine_name'
    Option -f pour affichage en temps réel

17. docker ps

    https://docs.docker.com/engine/reference/commandline/ps/

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
22. docker-machine ssh Aiur "docker swarm join --token $(docker swarm join-token worker -q) $(docker-machine ip Char):2377"
23. docker network create -d overlay overmind
24. docker service create --name orbital-command -e RABBITMQ_DEFAULT_USER=docker -e RABBITMQ_DEFAULT_PASS=dockerpass --network overmind rabbitmq
25. docker service ls
26. docker service create --name engineering-bay --replicas 2 -e OC_USERNAME=Docker -e OC_PASSWD=dockerpass --network overmind 42school/engineering-bay
27. docker service logs -f engineering-bay
28. docker service create --name marines --replicas 2 -e OC_USERNAME=Docker -e OC_PASSWD=dockerpass --network overmind 42school/marine-squad
29. docker service ps marines
30. docker service update --replicas 20 marines
31. docker service rm $(docker service ls -q)

    https://devopssec.fr/article/comprendre-gerer-manipuler-un-cluster-docker-swarm
    https://docs.docker.com/engine/swarm/

32. docker rm -f $(docker ps -aq)
33. docker rmi $(docker images -qa)
34. docker-machine rm Aiur -y

-- dockerfile --

https://putaindecode.io/articles/les-dockerfiles/
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

ex00

ex01

ex02

 Add any app compatible with ruby 2.5.1 and bundler 1.17.3, in my case I use a simple blog at https://github.com/acuD1/rails-docker-1-app.git

 $ git clone https://github.com/acuD1/rails-docker-1-app.git app

 $ docker build -t ft-rails:on-build .

 Create the Dockerfile from the subject

 $ echo -e 'FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]' > Dockerfile_Subject

 Then run subject build:

 $ docker build -t ex02 -f Dockerfile_Subject .

 $ docker run -it -p 3000:3000 --rm ex02
