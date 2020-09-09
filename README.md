
 ## Jenkins_CICD_Pipeline 
`Jenkins_CICD_Pipeline` automates continuous integration of html file committed on github repository called `Jenkins_CICD_Pipeline` and deploys static html page on AWS S3 bucket using jenkins file. This html file can be accessed from web browser. This project demonstrates my understanding and ability to create CICD pipeline using _Jenkins_. 

### Prerequisite
1. A little knowledge of basic commands in Unix terminal.
2. Understanding of software testing frameworks - JMeter and JUnit
3. Understanding of deployment strategies 

### Architecture of CICD 
![GitHub Logo](/images/cicd.png)
Format: ![Alt Text](url)


### Dependencies
##### 1. AWS account
You would require to have an AWS account to be able to build cloud infrastructure. Particularly, you will need to create S3 buckets, EC2 instances(Ubuntu 18.04 LTS amd64), and IAM users with policies:
AmazonEC2FullAccess
AmazonVPCFullAccess
AmazonS3FullAccess.

1) `Run` `chmod 400 your-private-key-file.pem` to set the permissions of your private key file (you downloded this file while creating your instance) so that only you can read it.
2) Launch EC2 instances. Enter into your EC2 instances via SSH.

If on mac or  linux, open terminal and SSH into:
`Run` ssh -i _your-private-key-file.pem_ ubuntu@ec2-_public ip or DNS_._region your instance is running_.compute.amazonaws.com

If on windows, ssh client (Putty)needs to be installed. Follow the guide:
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html#create-a-key-pair

##### 2. Jenkins file
You will need to create jenkins file that builds, prforms html linting, security scans and finally upload html file to AWS S3 bucket.
You can copy `Jenkinsfile` from this repo.

##### 3. Set up a GitHub Repository
A token needs to be generated. A link to https://github.com/settings/tokens/new?scopes=repo,read:user,user:email,write:repo_hook needs to be clicked to generate a token for Jenkins to use. You can select the default scopes in the opened link, that defines the access for a personal token for Jenkins.

Authenticate in Github, and add a note for what this token is (easier for later removal): "Jenkins Pipeline."

Make sure you copy the token 

##### 4. Jenkins on Ubuntu VM
You will need to install Jenkins and a few plugins to assist your requirements.

##### Steps:
1) Install Jenkins On Ubuntu: 
_Commands for installations_
`sudo apt update`
`sudo apt upgrade`
`sudo apt install default-jdk`

2) First, use `wget` to add the repo key to the system:
sudo wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -

3) When the key is added, the system returns OK. Next, append the Debian package repo address to the server's sources.list:
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

4) When both of these are all set, run update so that apt will use the new repo:
sudo apt update

5) install Jenkins and its dependencies:
sudo apt install jenkins

6) Since systemctl doesn't produce output, you can use its status command to confirm that Jenkins began successfully:
sudo systemctl status jenkins

7) Visit Jenkins on its default port, 8080, with your server IP address or domain name included like this: http://your_server_ip_or_domain:8080

8) You will see the "Unlock Jenkins" screen, displaying the location of the initial password. In the terminal, use `cat` to show the password:

`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

9) Copy and paste the 32-character alphanumeric password from the terminal into the Admin password field, then Continue.

10) The next screen gives you the choice of installing recommended plugins, or selecting specific plugins - choose the Install suggested plugins option, which quickly begins the installation process.

11) Start using Jenkins. 
##### Install 
    1) `Blue Ocean plugin ("BlueOcean aggregator")` from jenkins UI. 
    2) Run `sudo apt-get install -y tidy` to install the `tidy` linter in the server.
    3) `aquaMicroscanner `from jenkins UI.
    4) Run `sudo apt-get install -y docker.io` to install docker in the server.
    Add the docker group if it doesn't already exist:
    Run `sudo groupadd docker`
    Add your user (who has root privileges) to docker group.
    Run `sudo usermod -aG docker $USER`
    Run `sudo usermod -aG docker jenkins`
    5) `CloudBees Credentials` to install `AWS Credentials` and `S3 publisher` from jenkins UI. 
    Re-start jenkins running `sudo systemctl restart jenkins` 
    6) Set up AWS credentials in Jenkins:
    Credentials need to be created so that they can be used in our pipeline.
    ##### Steps:
        a) Go back to the main Jenkins page. Then click on the “Credentials” link from the sidebar.

        b) Click on "(global)" from the list, and then "Add credentials" from the sidebar.

        c) Choose "AWS Credentials" from the dropdown, add "aws-static" on ID, add a description like "Static HTML publisher in AWS," and fill in the AWS Key and Secret Access Key generated when the IAM role was created.

        d) Click OK, and the credentials should now be available for the rest of the system.
    7)  "Open Blue Ocean" link should show up in the sidebar. Click it, and it will take you to the "Blue Ocean" screen, where we will have to add a project. Click "create pipeline."

#####  Files included are
    1) `Jenkinsfile` 
    2) `index.html` HTML file to display as a web page
    3) `ScreenShots` folder- Having different stages of CICD pipeline







