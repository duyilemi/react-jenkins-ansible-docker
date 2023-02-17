## create reactjs app's jenkins pipline

## setup Jenkins on EC2

# install java and jenkins on EC2(ubuntu)

sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
 /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
 https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
 /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y

# or use user data

```
#!/bin/bash
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
###
```

# install nodejs and npm

```
sudo apt-get install -y curl
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

# give the jenkins user the privilige of the root user (sudo.....)

```
echo "jenkins ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
```

# ansible

- name: build and run container
  hosts: localhost
  become: yes
  become_user: charlie

  tasks:

  - name: stop running container
    command: docker stop react-nginx
    ignore_errors: yes

  - name: delete the container
    command: docker rm react-nginx
    ignore_errors: yes

  - name: delete the image
    command: docker rmi react-nginx
    ignore_errors: yes

  - name: build the image
    command: docker build -t react-nginx .
    args:
    chdir: /home/charlie/react-jenkins-ansible-docker

  - name: run the container
    command: docker run --name react-nginx -d -p 80:80 react-nginx

# exec command

cd /home/charlie/react-jenkins-ansible-docker ;
docker build -t react-nginx . ;
docker run --name react-nginx -d -p 80:80 react-nginx ;
