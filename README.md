## **TASK**
Implement a seamless CI/CD pipeline for Terraform using Jenkins in this project, enabling automated testing,
deployment, and management of infrastructure-as-code with ease

**STEP 1**

Create a Docker container that runs Jenkins and Terraform cli.

1. create Dockerifle: 
    - the official Jenkins image which can be found [here](https://hub.docker.com/r/jenkins/jenkin) was used as base for the Dockerfile, and additional commands were integrated in the code for Terraform

2. build the Jenkins image from Dockerfile

    ` docker build -t jenkins-server <path to dockerfile> `

3. Run the image into a Docker container 

    `docker run -d -p 8080:8080 --name jenkins-server jenkins-server`

4. Check that the container is running with `docker ps`


**STEP 2**

Set up Jenkins server

- for this you will need to retrieve the initial Jenkins admin password so you will need to access Jenkins directly inside the container:

    - Use `docker exec -it  <containerID or container name>  /bin/bash` to enter and execute commands inside the container  
    
    - After you run this you will find yourself in the `/app` directory which is the `WORKDIR` defined inside the Dockerfile

    - then run `cat /var/jenkins_home/secrets/initialAdminPassword` to retrieve the admin password

- Install suggested Jenkins plugins and finish set up

- In Jenkins install these plugins: GitHub integration, AWS credentials, and Terraform 


**STEP 3**

Set up Git repository with Terraform code

I used the code from https://github.com/dareyio/terraform-aws-pipeline.git which provisions Kubernetes cluster using EKS

Changes to make to this code:
- in `provider.tf` change the configuration to reflect your own s3 bucket
- push changes to GitHub
- Test that your configuration works by running Terraform init, plan and apply

**STEP 4**

Create credentials for GitHub and AWS secret and access keys

GitHub:

- create access token in GitHub
- within Jenkins, establish global credentials labeled as "Username with password," utilizing the access token as the password

AWS secrete and access keys:

- create credentials in Jenkins using the "AWS credentials" type
- for ID use `AWS_CRED`
