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
                     
