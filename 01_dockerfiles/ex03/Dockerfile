FROM debian:9.8

EXPOSE 443
EXPOSE 80
EXPOSE 22

ARG IP="0.0.0.0"

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y ca-certificates openssh-server wget postfix
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && \
    chmod +x script.deb.sh && ./script.deb.sh && \
    apt-get install -y gitlab-ce

# Create self signed certificate
RUN mkdir -p /etc/gitlab/ssl \
    && chmod +x /etc/gitlab/ssl \
    && openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -subj /C=FR/ST=Paris/L=Paris/O=42/OU=0/CN=${IP}/emailAddress=curquiza@student.42.fr \
    -keyout /etc/gitlab/ssl/selfsigned.key -out /etc/gitlab/ssl/selfsigned.crt \
    && echo "external_url \"https://${IP}\"" >> /etc/gitlab/gitlab.rb \
    && echo "nginx['redirect_http_to_https'] = true" >> /etc/gitlab/gitlab.rb \
    && echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/selfsigned.crt\"" >> /etc/gitlab/gitlab.rb \
    && echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/selfsigned.key\"" >> /etc/gitlab/gitlab.rb \
    && echo "gitlab_rails['gitlab_shell_ssh_port'] = 8022" >> /etc/gitlab/gitlab.rb

ENTRYPOINT service ssh start && (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && gitlab-ctl tail

# $> docker build -t ex03 --build-arg IP=$(docker-machine ip Char) .
# $> docker run -it --rm -p 80:80 -p 8022:22 -p 443:443 --privileged ex03

# Tests :
# 1) go to https://$(docker-machine ip Char) and register (it could be really slow: reload when getting 502)
# NB : the reset password page (first page) creates a user 'root' with the password you entered

# 2) HTTPS :
# clone : GIT_SSL_NO_VERIFY=true git clone https://$(docker-machine ip Char):8000/root/<repo>.git
# push : GIT_SSL_NO_VERIFY=true git push origin master

# 3) SSH :
# Add your ssh key in gitlab
# clone : GIT_SSL_NO_VERIFY=true git clone ssh://git@$(docker-machine ip Char):8022/root/<repo>.git
# push : GIT_SSL_NO_VERIFY=true git push origin master