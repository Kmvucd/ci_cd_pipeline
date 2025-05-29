# Xray lung classifier

Data link: https://drive.google.com/file/d/1pfIAlurfeqFTbirUZ5v_vapIoGPgRiXY/view?usp=sharing


## Workflows

- constants
- config_enity
- artifact_enity
- components
- pipeline
- main


# pipeline

## steps to follow


In EC2, write "sudo apt-get update -y" -- updating

then write "sudo apt-get upgrade -y"

install dockers --> curl -fsSL https://get.docker.com -o get-docker.sh

To list the file --> ls

Executing file --> sudo sh get-docker.sh

Once it is executed, write "docker" 

Give permissions --> sudo usermod -aG docker ubuntu

Create group --> newgrp docker

Now check for images --> docker images

To check any containers running or not --> docker ps -a

By this we have configured docker on the linux

Now we need to push image to ECR using docker and from EC2 write a code so that we can pull image from ECR
Go to github-> settings --> Actions --> Runners --> Go down to download and execute each commands in EC2
Go down to configure and execute each commands in EC2

And then enter for default values

Then run --> ./run.sh -- Done with the github connections

Next we need to edit inbound rule in EC2:

EC2-> instances-> select instance -> security --> security groups --> edit inbound rules --> add rule --> port is 8501 adn select local as 0.0.0.0/0 --> save rules

Now again try to push code from local to git (git add . ,,, git push -u origin main). As we have added all secrets in github, now if we go into actions->workflows, it will be running all the steps
completely

