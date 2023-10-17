# Creating a Jenkins Environment

![Jenkins diagram.png](images%2FJenkins%20diagram.png)

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

Do the same for NodeJS, and Office 365 Connector <br>

Once done we need to configure NodeJS settings so go on 'Manage Jenkins' then click 'Tools'

![Screenshot 2023-10-17 134751.png](images%2FScreenshot%202023-10-17%20134751.png)

Scroll all the way to the bottom till you see 'NodeJS Installation'

We want the latest version 12, so we select and Save

![Screenshot 2023-10-17 135123.png](images%2FScreenshot%202023-10-17%20135123.png)




## Step 2 (Trigger the Job)

- In the build trigger we only want 'shaluo-CD' to work if the merge is successful

![Screenshot 2023-10-13 132338.png](images%2FScreenshot%202023-10-13%20132338.png)
- We want to only build this project once the merge is successful
- We also want to use a webhook to Github

## Step 3 (Jenkins SSH)

![Screenshot 2023-10-13 132442.png](images%2FScreenshot%202023-10-13%20132442.png)
- Provide the .pem file so Jenkins can SSH into the VM

## Step 4 (Get Nginx Installed)


- Use a build step in 'shaluo-CD' to copy the application code
- Lets first start with getting Nginx on to our Virtual Machine
```
rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@54.171.236.179:/home/ubuntu
ssh -o "StrictHostKeyChecking=no" ubuntu@54.171.236.179 <<EOF
	sudo apt-get update -y
    sudo apt-get upgrade -y
    sudo apt-get install nginx -y
    sudo systemctl restart nginx
    sudo systemctl enable nginx
EOF
```
Manually use the 'Build Now' to test if it works

## Step 5 (Copy the app code)

Create a new job called `shaluo-app` <br>

Here we are going to do the app deployment

Build the trigger so this job runs after `shaluo-CD`

Keep everything else the same

- Lets get `node.js`, `npm` and `pm2` installed
- Change the directory to the app
- Install `npm` in the app folder
- Use `pm2` to start the app in the background

```
ssh -A -o "StrictHostKeyChecking=no" ubuntu@54.171.236.179 <<EOF

curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt install nodejs -y

sudo npm install pm2 -g

cd app

npm install

pm2 start  app.js

pm2 restart app.js
```

![Screenshot 2023-10-11 114847.png](images%2FScreenshot%202023-10-11%20114847.png)



























