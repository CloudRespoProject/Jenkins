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
        Download Jenkins :   wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
        sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' --- append the Debian 
        package repository address to the server’s sources.list
        sudo apt update
        sudo apt install jenkins

Start Jenkins : sudo systemctl start jenkins
                sudo systemctl status jenkins

From the server terminal sudo cat /var/lib/jenkins/secrets/initialAdminPassword --- To get the jenkins password. 
Configure Jenkins on the Server : http://serverIPORDomain:8080
![image](https://github.com/CloudRespoProject/Jenkins/assets/144565485/9f7c6a6c-cb7e-449f-ac10-edcf73d88ee3)






                     
