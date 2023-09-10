Setting up a CI/CD pipeline by integrating Jenkins with AWS CodeBuild and AWS CodeDeploy
Jenkins open-source automation server is used to deploy AWS CodeBuild artifacts with AWS CodeDeploy.
When developer commit any changes in the code, it triggers the CI/CD pipeline and then code changes pushed to GitHub repo, automatically pused to AWS CodeBuild and then the output is deployed on CodeDeploy. 
![github](https://github.com/CloudRespoProject/Jenkins/assets/144565485/ab887e1c-cd44-4f5b-8a55-3b217dfa0439)
Steps to build the pipeline
Step 1: Building the infrastructure including jenkins server, setting up Code Build and Code Deploy.
Step 2: Accessing and configuring the Jenkins server. 
Step 3: Creating a project and configuring the CodeDeploy Jenkins plugin.
Step 4: Testing the CI/CD Pipeline. 

Step 1 : Build the infrastructure:
Amazon S3 bucket—Stores the GitHub repository files and the CodeBuild artifact application file that CodeDeploy uses. 
Create 2 IAM roles : 1. S3 bucket role which contain a policy that allows the Jenkins server access to the S3 bucket.
                     2. JenkinsRole — An IAM role and instance profile for the Amazon EC2 instance for use as a Jenkins server. This role 
                        allows Jenkins on the EC2 instance to access the S3 bucket to write files and access to create CodeDeploy deployments. 
                     3. CodeDeploy service role— An IAM role to enable CodeDeploy to read the tags applied to the instances or the EC2 Auto 
                        Scaling group names associated with the instances.
                     4. CodeDeployRole— An IAM role and instance profile for the EC2 instances of CodeDeploy. This role has permissions to write files to the S3 bucket. 
                     5. CodeBuildRole—  An IAM role to be used by CodeBuild to access the S3 bucket and create the build projects.
                     
Create a Jenkins server— An EC2 instance running Jenkins.
CodeBuild project—This is configured with the S3 bucket and S3 artifact.
Auto Scaling group—Contains EC2 instances running Apache and the CodeDeploy agent fronted by an Elastic Load Balancer. Auto Scaling launch configurations—For use by the Auto Scaling group. Security groups—For the Jenkins server, the load balancer, and the CodeDeploy EC2 instance.

Install and Configure Jenkins server on EC2 Ubuntu 20.04
        
Download Jenkins :  sudo wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
        
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' --- append the Debian package repository address to the server’s sources.list
        
sudo apt update
        
sudo apt install jenkins


Start Jenkins : sudo systemctl start jenkins
                sudo systemctl status jenkins



 


Configure Jenkins on the Server : 
From the server terminal sudo cat /var/lib/jenkins/secrets/initialAdminPassword --- To get the jenkins password.

http://serverIPORDomain:8080

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/9f7c6a6c-cb7e-449f-ac10-edcf73d88ee3)

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/3ea2ae3a-3068-4b86-b387-bfda90bb1066)

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/9eff4c70-0bf4-447f-be16-f84c55e926f4)

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/60e995bd-9474-48f7-a5ac-8cdc6b8e0f99)



Create a project and configure the CodeDeploy Jenkins plugin

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/31515fc7-e645-45de-ad34-b04dfe1b7d1e)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/5f222bee-469c-40a1-b18d-a7eeecc3b372)

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/420493bf-4591-4e92-9023-884877d2ba39)

Create a project and configure the CodeDeploy Jenkins plugin. 

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/b4617112-7476-4f1b-84ad-0f669c6d6cba)
                     
![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/c0f82611-695d-41cb-8196-75e7f6842c7c)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/4c65c2f3-ca6e-4d1f-8f18-8ef8cb32fbd4)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/50d002b2-8445-42f3-87d2-ec9cbb3fe83d)

POLL SCM : To periodically check for new commits to your repository set the the cron job for periodically updates.

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/707d9413-65ab-420a-be06-dabf937e57d0)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/5579749a-1e79-43d8-8a47-807bab2c2739)

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/9bc49ad3-3f49-48b2-95df-0e19cf5ba7b4)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/539ced56-94cb-4102-89c0-e7b1f7451cf9)

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/89d53a84-88d1-4261-9ed7-e38d07d11a87)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/9afc8e95-c44d-4d77-8fa4-a379de5eb36d)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/d682d773-c7f9-4717-8d52-19639d8b1c82)

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/87b90d0e-23ec-46bf-a47b-56affce30b8b)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/cadd2ac3-af05-4761-b249-bd7199159923)


![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/78af41b7-4fba-480e-83a8-bf730fb864bc)



Testing the whole CI/CD pipeline

To test the whole solution, put an application on your GitHub repository.

The following screenshot shows an application tree containing the application source files, including text and binary files, executables, and packages:

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/fb8d1fe3-25f7-4a27-8f5d-241c9009bef4)

In this example, the application files are the templates directory, test_app.py file, and web.py file.

The appspec.yml file is the main application specification file telling CodeDeploy how to deploy your application. Jenkins uses the AppSpec file to manage each deployment as a series of lifecycle event “hooks”, as defined in the file. For information about how to create a well-formed AppSpec file, see AWS CodeDeploy AppSpec File Reference.

The buildspec.yml file is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build. You can include a build spec as part of the source code, or you can define a build spec when you create a build project. For more information, see How AWS CodeBuild Works.

The scripts folder contains the scripts that you would like to run during the CodeDeploy LifecycleHooks execution with respect to your application requirements. For more information, see Plan a Revision for AWS CodeDeploy.

To test this solution, perform the following steps:

Unzip the application files and send them to your GitHub repository, run the following git commands from the path where you placed your sample application:

$ git add -A

$ git commit -m 'Your first application'

$ git push On the Jenkins server dashboard, wait for two minutes until the previously set project trigger starts working. After the trigger starts working, you should see a new build taking place.

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/839fbeb0-75f7-4de3-add6-f6603c2e4c1b)


In the Jenkins server Console Output page, check the build events and review the steps performed by each Jenkins plugin. You can also review the CodeDeploy deployment in detail, as shown in the following screenshot:

![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/5acb81b6-e512-4474-adca-6a0b1dcacc14)

On completion, Jenkins should report that you have successfully deployed a web application. You can also use your ELBDNSName value to confirm that the deployed application is running successfully.

