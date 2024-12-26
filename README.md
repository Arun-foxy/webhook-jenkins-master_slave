we need 3 EC2 instance to achive this flow

User --> Git --> Jenkins --> SonarQube (Code Analysis) --> Docker --> EndUser

Content CSS link https://www.free-css.com/free-css-templates/page296/oxer

Now we have push our code github below steps
    * We have repo or  (we need create repo)
    * git init
    * git add .
    * git commit -m "commit details"
    * git remote add origin link
    * git checkout -b your_branch_name
    * git push -u origin branch_name
# Now we have launch 3 Instance  1. Jenkins 2. SonarQube 3. Docker
    Note : All the machine required more useage and storage so we go with t2.medium for testing process
    # Instances 
        1. Instances name --> Jenkins, SonarQube, Docker
        2. Select AMI as per request 
        3 Instances type t2.medium
        4. key-pair 
    # Jenkins Instance
        # amazon machine jenkins install https://medium.com/@Raghvendra_Tyagi/how-to-install-and-configure-jenkins-on-amazon-linux-2024-and-building-your-first-project-a-97a1fe754216
        # Java required need to run Jenkins
        # https://www.jenkins.io/doc/book/installing/linux/
        # Jenkins run on port 8080 need aollow this
        # cat /var/lib/jenkins/secrets/initialAdminPassword 
        # Configure the webhook and enable Pull & Push on and test the usecase fine or not
    # Sonaqube (t2.medium 2CPU 4gb memory)
        # Java required need to run Sonarqube
        # hostnamectl set-hostname sonarqube -> change hostname
        # /bin/bash
        #
        # sudo apt install openjdk-11-jre
        # wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
        # sudo apt install unzip
        #   file_name 
        # cd sonar_unzip_filename & cd bin & ls & cd linux (move to file based on AMI selection)
        # ./sonar.sh  & ./sonar.sh console


        # amazon machine
        # amazon-linux-extras install epel
        # amazon-linux-extras install java-openjdk11
        # SonarQube (https://binaries.sonarsource.com/?prefix=Distribution/sonarqube/)
        # SonarQube https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.6.zip
        # uzip file name
        # cp -R file-name /opt  & cd opt
        # Create user
            useradd sonar
            passwd sonar
        # open visudo (file)
        # Add sonar user in wheel group
            Line 108 :
                %wheel  ALL=(ALL)   ALL
                sonar   ALL=(ALL)   ALL
            We have done same for Nopassword here just belowmention
                 sonar   ALL=(ALL)   NOPASSWD: ALL
        # cd /opt
        # chown -R sonar:sonar sonarqube-7.6(unzip file name)
        # cd sonarqube-7.6 & cd bin & cd linux-86*64 
        # sudo su - sonar & id 
        # netstat -tulpn|grep 9000



WebHook
=======
    # Push code-to github 
    # login jenkins create job are already existing job there configure on that.
        * Select the pipeline type
        * Description --> About project we can mention any thing
        * Select build type (Github project provide Url of our repo)



master
#!/bin/bash

# Update all packages
sudo yum update -y

# Install Java 17 (Amazon Corretto)
sudo yum install java-17-amazon-corretto -y

# Add the Jenkins repository
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

# Import the Jenkins repository GPG key
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# Install Jenkins
sudo yum install jenkins -y

# Enable Jenkins service to start on boot
sudo systemctl enable jenkins

# Start Jenkins service
sudo systemctl start jenkins

# Optionally, check Jenkins status (to confirm it's running)
sudo systemctl status jenkins


slave -1 
#!/bin/bash
sudo yum update -y
sudo yum install java-17-amazon-corretto  -y
sudo yum install docker -y
sudo systemctl enable docker
sudo systemctl start docker
sudo yum install git -y
sudo mkdir /home/ec2-user/jenkins
cd /home/ec2-user/jenkins
curl -sO http://54.172.174.135:8080/jnlpJars/agent.jar
java -jar agent.jar -url http://54.172.174.135:8080/ -secret 59b0a112fb05723a5aeb9fcf5933a08eacb4d1bd8e34a4210b37aa54ed1517c5 -name slave -webSocket -workDir "/home/ec2-user/jenkins"


use the token for webhook 403 error 
https://www.youtube.com/watch?v=wtdbReb7r7s






