Setting up a CI/CD pipeline by integrating Jenkins with AWS CodeBuild and AWS CodeDeploy
Jenkins open-source automation server is used to deploy AWS CodeBuild artifacts with AWS CodeDeploy.
When developer commit any changes in the code, it triggers the CI/CD pipeline and then code changes pushed to GitHub repo, automatically pused to AWS CodeBuild and then the output is deployed on CodeDeploy. 
The functioning pipeline creates a fully managed build service that compiles your source code. It then produces code artifacts that can be used by CodeDeploy to deploy to your production environment automatically.

