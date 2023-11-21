<div>
  <h1 align="center"><b>Deploying a YouTube Clone App with DevSecOps and <br/>Jenkins Shared Library</b></h1>
  <img src="./public/assets/CICD-Project-1.png" alt="CICD-Project-1.png">
  <p><b>This blog is your gateway to a secure DevSecOps pipeline for your project. With Kubernetes, Docker, SonarQube, Trivy, OWASP Dependency Check, Prometheus, Grafana, Jenkins (and a shared library), Splunk, Rapid API, Slack notifications, and efficient parameters for creating and destroying your environment, we've got you covered.</b></p>
  <h4>
      <b>
        <u>
          <a href="https://github.com/Shravankumar1989/Youtube-clone-app.git">
            Click here for the GitHub repository.
          </a>
        </u>
      </b>
  </h4>
  <h4>
    Please follow the steps below.
  </h4>
  <p><b>Step 1 - </b>Launch an Ubuntu 22.04 instance for Jenkins</p>
  <p><b>Step 2.1 - </b>Install Docker on the Jenkins machine</p>
  <p><b>Step 2.2 - </b>Install Trivy on Jenkins machine</p>
  <p><b>Step 3.1 - </b>Launch an Ubuntu instance for Splunk</p>
  <p><b>Step 3.2 - </b>Install the Splunk app for Jenkins</p>
  <p><b>Step 4.1 - </b>Integrate Slack for Notifications</p>
  <p><b>Step 4.2 - </b>Install the Jenkins CI app on Slack</p>
  <p><b>Step 5.1 - </b>Start Job</p>
  <p><b>Step 5.2 - </b>Create a Jenkins shared library in GitHub</p>
  <p><b>Step 5.3 - </b>Add Jenkins shared library to Jenkins system</p>
  <p><b>Step 5.4 - </b>Run Pipeline</p>
  <p><b>Step 6.1 - </b>Install Plugin</p>
  <p><b>Step 6.2 - </b>Configure Java and Nodejs in Global Tool Configuration</p>
  <p><b>Step 6.3 - </b>Configure Sonar Server in Manage Jenkins</p>
  <p><b>Step 6.4- </b>Add New stages to the pipeline</p>
  <p><b>Step 7 - </b>Install OWASP Dependency Check Plugins</p>
  <p><b>Step 8.1 - </b>Docker Image Build and Push</p>
  <p><b>Step 8.2 - </b>Create an API key from Rapid API</p>
  <p><b>Step 8.3 - </b>Run the Docker container</p>
  <p><b>Step 9.1 - </b>Kubernetes Setup</p>
  <p><b>Step 9.2 - </b>Kubectl is to be installed on Jenkins</p>
  <p><b>Step 9.3 - </b>Kubernetes Master-Slave setup</p>
  <p><b>Step 9.4 - </b>Install Helm & Monitoring K8S using Prometheus and Grafana</p>
  <p><b>Step 9.5 - </b>Kubernetes Deployment</p>

  <h2><b>Step 1 - Launch an Ubuntu 22.04 instance for Jenkins</b></h2>
  <p><b>Log into AWS Console: Sign in to your AWS account.</b></p>
  <p><b>Launch an Instance:</b></p>
  <p><b>Choose "EC2" from services. Click "Launch Instance."</b></p>
  <p><b>Choose an AMI: Select an Ubuntu image.</b></p>
  <p><b>Choose an Instance Type: Pick "t2.large."</b></p>
  <p><b>Key Pair: Choose an existing key pair or create a new one.</b></p>
  <p><b>Configure Security Group: Create a new security group. Add rules for HTTP, and HTTPS, and open all ports for learning purposes.</b></p>
  <img src="./public/assets/EC2-Instance-2.png" alt="EC2-Instance-2.png">
  <p><b>Add Storage: Allocate at least 20 GB of storage.</b></p>
  <p><b>Launch Instance: Review and launch the instance.</b></p>
  <p><b>Access Your Instance: Use SSH to connect to your instance with the private key.</b></p>
  <p><b>Keep in mind, that opening all ports is not recommended for production environments; it's just for educational purposes.</b></p>
  <img src="./public/assets/EC2-Instance-1.png" alt="EC2-Instance-1.png">
  <p><b>Connect to Your EC2 Instance and Install Jenkins:</b></p>
  <p><b>Use MobaXterm or PuTTY to connect to your EC2 instance. Create a shell script named</b></p>
  
  ```sh
  # Open the file 'install_jenkins.sh' in the 'vi' editor with superuser privileges
  sudo vi install_jenkins.sh
  ```

  ```sh
  #!/bin/bash
  # Update the package lists for upgrades and new package installations
  sudo apt update -y
  
  # Install the OpenJDK version 11 JDK package
  sudo apt install openjdk-11-jdk -y
  
  # Download the Jenkins repository signing key and save it to '/usr/share/keyrings/jenkins-keyring.asc'
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  
  # Add the Jenkins repository to the system's software sources list
  echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
  
  # Update the package lists to include packages from the newly added Jenkins repository
  sudo apt-get update -y

  # Install Jenkins from the added repository
  sudo apt-get install jenkins -y
  
  # Start the Jenkins service
  sudo systemctl start jenkins
  
  # Check and display the status of the Jenkins service
  sudo systemctl status jenkins
  ```
  
  <p><b>Save and exit the text editor.</b></p>
  <p><b>Make the script executable:</b></p>
  
   ```sh
  # Grant execute permission to the 'install_jenkins.sh' script
  sudo chmod +x install_jenkins.sh
  ```
  <p><b>Run the script:</b></p>
  
  ```sh
  # Execute the 'install_jenkins.sh' script
  ./install_jenkins.sh
  ```
  <p><b>The script will install Jenkins and start the Jenkins service.</b></p>
  <p><b>You will need to go to your AWS EC2 Security Group and open Inbound Port 8080 since Jenkins works on Port 8080.</b></p>
  <p><b>Now, grab your Public IP Address</b></p>
  
  ```sh
  # This line seems to be an indication to check Jenkins on the provided EC2 public IP at port 8080
  # <EC2 Public IP Address:8080>
  # Display the initial admin password for Jenkins
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```

  <img src="./public/assets/Jenkins-1.png" alt="Jenkins-1.png">
  <p><b>Now, install the suggested plugins.</b></p>
  <img src="./public/assets/Jenkins-3.png" alt="Jenkins-3.png">
  <p><b>Jenkins will now get installed and install all the libraries.</b></p>
  <p><b>Create an admin user</b></p>
  <img src="./public/assets/Jenkins-5.png" alt="Jenkins-5.png">
  <p><b>Click on save and continue.</b></p>
  <p><b>Jenkins Dashboard</b></p>
  <img src="./public/assets/Jenkins-8.png" alt="Jenkins-8.png">

  <h2><b>Step 2.1 - Install Docker on the Jenkins machine</b></h2>
  <p><b>Run the below commands to install the docker</b></p>
  
  ```sh
  # Update the package lists for upgrades and new package installations
  sudo apt-get update
  
  # Install Docker
  sudo apt-get install docker.io -y
  
  # Add the current user (assumed to be 'ubuntu') to the 'docker' group
  sudo usermod -aG docker $USER
  
  # Apply the new group membership without logging out and back in
  newgrp docker
  
  # Change the permissions of the Docker socket to allow all users to access it
  sudo chmod 777 /var/run/docker.sock
  ```

  <p><b>After the docker installation, we will create a Sonarqube container (Remember to add 9000 ports in the security group).</b></p>
  <p><b>Run this command on your EC2 instance to create a SonarQube container:</b></p>

  ```sh
  # Run SonarQube in a Docker container, mapping port 9000 on the host to port 9000 in the container
  docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
  ```
  <p><b>Now copy the IP address of the ec2 instance</b></p>

  ```sh
  # This line seems to be an instruction to access SonarQube on the provided EC2 public IP at port 9000
  # <ec2-public-ip:9000>
  ```

  <img src="./public/assets/SonarQube-1.png" alt="SonarQube-1.png">
  <p><b>Enter username and password, click on login and change password</b></p>

  ```sh
  # These lines indicate the default SonarQube credentials, which are typically changed during initial setup
  # username admin
  # password admin
  ```
  <img src="./public/assets/SonarQube-2.png" alt="SonarQube-2.png">
  <p><b>Update New password, This is Sonar Dashboard.</b></p>
  <img src="./public/assets/SonarQube-3.png" alt="SonarQube-3.png">


  
  <p><b></b></p>
  <p><b></b></p>
  <p><b></b></p>
  <p><b></b></p>
  <p><b></b></p>
  
  
</div>
