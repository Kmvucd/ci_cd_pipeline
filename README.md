# Xray lung classifier

Data link: https://drive.google.com/file/d/1pfIAlurfeqFTbirUZ5v_vapIoGPgRiXY/view?usp=sharing

This is the clone of dlproject1, where it folks that repository just to explain the way of doing CI/CD pipelines from source code to github using github actions and then to docker and then to AWC EC2, once it is done, I can use local host from EC2 to run my application.

## Workflows

- constants
- config_enity
- artifact_enity
- components
- pipeline
- main


# pipeline
CI - Continuous Integration
CD1 - Continuous Delivery
CD2 - Continuous Deployment

CI, CD1 are performing through github actions server.
CD2 is perfoming through own machine (self host server) in AWS EC2. Self hoster I need to configure.

## How workflow is happening?
CI Phase (CI+CD1):
    Once developer pushes the code to github, it triggers github actions (CI/CD pipelines) via main.yaml file. Github action runs 
        1. Docker build: Packages code and dependencies into docker image.
        2. Docker push: Pushes image to AWS ECR.
CD Phase (CD2):
    On a target machine like AWS EC2, docker pulls image from ECR. Docker runs the containers

## steps to follow

For the workflow and integration, Once main.yaml file is build, we need to create secrets and launch instance in EC2.

Go to AWS EC2 -> Launch Instance -> Give Name(CNNApp) -> Select ubuntu -> Select Payless -> Select instance type as t2 medium (payless) -> Create new key pair -> Name(CNNApplication) -> RSA-> .pem -> Create -> Allow HTTPS, Allow HTTP both along with Allow SSH (0.0.0.0/0)-> Configure storage as 16(upto 30 I can select anything)-> Launch(Successfully launched)

Once launched, go into launched instance -> Select my instance -> Connect to instance -> Connect(Now I can access the EC2 terminal).

Now go into IAM-> Users-> Create access keys -> Give admin access ->Download access keys (If not downloaded already).

Now we need to add secrets in github which are in main.yaml:
Go into github -> settings -> secrets -> Actions -> Repository secrets -> Give name and secret -> Add secret. Similarly Add Secret for all (DOCKER_USERNAME , DOCKER_PASSWORD, IMAGE_NAME, REGISTRY, AWS_ACCESS_KEY, AWS_SECRET_ACCESS_KEY, AWS_REGION).

In EC2, for updating write "sudo apt-get update -y" 
then write "sudo apt-get upgrade -y"

install dockers --> curl -fsSL https://get.docker.com -o get-docker.sh

To list the file --> ls

Executing file --> sudo sh get-docker.sh

Once it is executed, write "docker" 

Give permissions --> sudo usermod -aG docker ubuntu

Create group --> newgrp docker

Now check for images --> docker images

To check any containers running or not --> docker ps -a
If we want to stop any container, "docker stop 'container id from above query'"
To remove container docker rm "'container id'"
By this we have configured docker on the linux

Now we need to push image to ECR using docker and from EC2 write a code so that we can pull image from ECR

Go to github-> settings --> Actions --> Runners --> Go down to download and execute each commands in EC2
Go down to configure and execute each commands in EC2

And then enter for default values

Then run --> ./run.sh -- Done with the github connections

Next we need to edit inbound rule in EC2:

EC2-> instances-> select instance -> security --> security groups --> edit inbound rules --> add rule --> port is 8501 adn select local as 0.0.0.0/0 --> save rules

Now again try to push code from local to git (git add . ,,, git push -u origin main). As we have added all secrets in github, now if we go into actions->workflows, it will be running all the steps
completely.

Go to ci_cd_pipeline repository-> actions-> Check the process of integration and deployment. Once first two steps of Integrations and Delivery is done, go to Settings->Actions->Runner and check for the current one. If it is changed from idle to offline status, we need to activate it.

We can activate this by, Go to EC2 instance and then click on connect. Commands to activate is,

1. ls
2. cd actions-runner/
3. ls --> check .sh file is there or not
4. ./run.sh

Now Github is connected and now if I go back to check runner, it will changed to Active from offline.

But if deployment is failed, again do re run failed jobs or go to EC2 instance and run ./run.sh

Once everything is completed successfully, go to EC2 instance and copy public ipv4 address and paste it as url with :port ie.,(13.14.165.10:8501). Then the final application of streamlit will be displayed.

## Conclusion

In this project, I have implemented CI/CD pipeline end to end, but unittest or pytest is missing in this file.
