# Creating a Jenkins Environment

## Step 1: Launch an Instance

Launch a new Instance call it 'tech254-shaluo-jenkins' <br>

Select the Ubuntu 20.04 LTS AMI 

![Screenshot 2023-10-13 171123.png](Screenshot%202023-10-13%20171123.png)

- We can leave the instance type as 't2.micro' and add the SSH key 'tech254'

Under network settings, we want to select our VPC, and make sure the subnet is public so we can access it. <br>
Enable Auto-assign public IP and now we need to select a security group

![Screenshot 2023-10-13 171539.png](Screenshot%202023-10-13%20171539.png)

Make sure you have these 4 inbound rules

![Screenshot 2023-10-13 171834.png](images%2FScreenshot%202023-10-13%20171834.png)
- Port 8080: This is the port to allow access to Jenkins

**Launch the Instance and SSH into it using GitBash**

## Step 2: Install Java 

We need to install Java before we install Jenkins as Jenkins works on Java <br>

**Input the below commands one by one**

```
sudo apt update

sudo apt install default-jdk

java -version
```

## Step 3: Install Jenkins

We can now install jenkins so use these commands:

**Input the commands one by one**
```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

sudo apt update

sudo apt install jenkins

sudo systemctl start jenkins

sudo systemctl enable jenkins
```

Jenkins should now be installed

## Step 4: Access Jenkins

Open a web browser and copy your and copy and paste your Public IP with the extension :8080 at the end <br>
`<http://your_server_ip_or_domain>:8080`

![Screenshot 2023-10-13 174202.png](images%2FScreenshot%202023-10-13%20174202.png)

We need to get the administrator password from our ec2 instance using this command:

`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

Copy and paste the password and click 'Continue'

We will be taken to the 'Customize Jenkins' screen

![Screenshot 2023-10-13 174449.png](images%2FScreenshot%202023-10-13%20174449.png)

I have used the suggested ones, but we will add some plugins that we will need and I will walk you through that

## Step 4b:

To add plugins, click 'Manage Jenkins' and click 'Plugins'

![Screenshot 2023-10-13 175121.png](images%2FScreenshot%202023-10-13%20175121.png)

Search for the plugin required and click 'install'

![Screenshot 2023-10-13 175350.png](images%2FScreenshot%202023-10-13%20175350.png)

Once installation is successful, restart jenkins

![Screenshot 2023-10-13 175508.png](images%2FScreenshot%202023-10-13%20175508.png)





























