# CICD Flow

I've worked on  various Jenkins pipelines to streamline CI/CD processes within our organization. Given the different  range of applications, I've created CI/CD pipelines to suit the specific needs of each application. These pipelines incorporate various build tools such as Maven, Sonar, Docker, Tomcat and others, ensuring seamless integration and deployment workflows..

·  The CI process will consist of the following stages:

o  Check out the source code in the Jenkins workspace.

o  Perform the sonar scan; if the scan result is successful, build will proceed with other activities in the job; otherwise, the job will be aborted.

o  Create the binaries (war, docker image,.exe) with respect to build tools.

o  publish the binaries (war, docker image,.exe) to the JFrog artifactory

·  Once these activities are completed, we can say the CI part is complete and will start the CD part.

·  We will initiate the CD job from the CI job in the post-build section. The CI job will trigger the CD job with parameters.

## <sup>**CD job for deploying to Kubernetes Clusters**</sup>

* Here we will checkout the git repository, which contains our manifest file.
* Update the manifest file with the latest image tag.
* We will login to the Kubernetes clusters with an IAM user.
* Deploy the application with the help of Kubectl commands on clusters.

·   Once the deployment is done, we will verify the deployment.

·   If the deployment is not successful, we will check the application logs and try to fix the issue.

·   If we are unable to fix the issue, we will roll back the application to the previous version.

&#x20;

## CD job for deploying to Tomcat

&#x20;

* Here we will checkout the git repository, which contains our ansible playbook.

·  Playbook contains below  tasks,

o   download the binaries to the tomcat server and extract the binaries.

o  stop the tomcat services.

o  take the backup of an existing binaries and configuration files.

o  Copy the latest binaries, configuration files and dependencies to the targeted path.

o   start tomcat service and verify the deployment.

o  If a deployment fails, we'll first examine the application logs to identify and address any issues. If possible, we'll fix the problem; otherwise, we'll initiate a rollback to the previous version of the application.

\


### Kubernetes deployment steps&#x20;

Jenkinsfile to define the steps in a pipeline for deploying a spring-boot application to an EKS cluster using Jenkins.

#### The steps should include 

* Checking out the git repository,&#x20;
* Building a Jar,&#x20;
* Building a Docker image,&#x20;
* Pushing the image to ECR,&#x20;
* Integrating Jenkins with the EKS cluster,&#x20;
* deploying an app to EKS.&#x20;

To do this, select “Pipeline script” under the pipeline section and specify the necessary steps.

```
pipeline {
   tools {
       maven 'Maven3'
   }
   agent any
   stages {
       stage('Checkout') {
           steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: '<GIT_REPO_URL>']]])
           }
       }
       stage('Build Jar') {
           steps {
               sh 'mvn clean package'
           }
       }
       stage('Docker Image Build') {
           steps {
               sh 'docker build -t <IMAGE_NAME> .'
           }
       }
       stage('Push Docker Image to ECR') {
           steps {
               withAWS(credentials: '<AWS_CREDENTIALS_ID>', region: '<AWS_REGION>') {
                   sh 'aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <ECR_REGISTRY_ID>'
                   sh 'docker tag <IMAGE_NAME>:latest <ECR_REGISTRY_ID>/<IMAGE_NAME>:latest'
                   sh 'docker push <ECR_REGISTRY_ID>/<IMAGE_NAME>:latest'
               }
           }
       }
       stage('Integrate Jenkins with EKS Cluster and Deploy App') {
           steps {
               withAWS(credentials: '<AWS_CREDENTIALS_ID>', region: '<AWS_REGION>') {
                 script {
                   sh ('aws eks update-kubeconfig --name <EKS_CLUSTER_NAME> --region <AWS_REGION>')
                   sh "kubectl apply -f <K8S_DEPLOY_FILE>.yaml"
               }
               }
       }
   }
   }
}
```



If look in detail about the pipeline has five stages:

1. Checkout: The stage “Checkout” retrieves the code from a Git repository. It specifies the Git repository URL and the branch to checkout (in this case, the main branch). The code will be checked out in the Jenkins workspace and available for the rest of the pipeline to use.
2. Maven Build: This build stage of the pipeline is responsible for creating a jar file of the spring boot application code. This is done using the Apache Maven build tool. The steps in this stage include:

i) Cleaning any previous build artifacts using the command mvn clean.\
ii) Building the jar file using the command mvn package.

\
This stage compiles the source code into a standalone executable jar file, which will be used in later stages of the pipeline.

Building a JAR file is a one-time process, but it may need to be rebuilt if changes are made to the code or if a new version is being released. Additionally, JAR files can also be repackaged if needed.

3\. Docker Image Build: This stage is for building a Docker image using a Dockerfile. The stage runs a shell command using the sh step, which runs the docker build command. The -t option is used to specify the name of the image, and the . at the end of the command specifies that the build context is the current directory. The image name is specified using the placeholder \<IMAGE\_NAME>.

4\. Push Docker Image to ECR: This stage pushes the Docker image that was built in the previous stage to Amazon Elastic Container Registry (ECR), which is a fully-managed Docker container registry service provided by AWS. The stage uses AWS CLI to authenticate and push the image to the specified ECR repository.

5\. Integrate Jenkins with EKS and deploy: This stage integrates Jenkins with an AWS EKS (Elastic Kubernetes Service) cluster and deploys an application. By using the withAWS block, Jenkins can securely access AWS resources (such as Amazon Elastic Container Registry or Amazon Elastic Kubernetes Service) on behalf of the user.

* The first command updates the kubectl configuration to connect to the specified EKS cluster.
* Then, the application is deployed to the cluster using the kubectl apply command and the YAML file for deployment and service.

#### Trigger the pipeline

Start the pipeline by clicking “Build Now” in the Jenkins Dashboard

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfafcTB0_kJamzesOW4G8TZKBjBsZB2sC3piBw9ewRg1utizqOLINZDNpbamr2uiXEKbdBMs8UDC-QCnf26Rzk7WoNn3a2KySUaDfOmT60vgzhs44-uUTpCBPlJ44RnjngAnvCJeDjH490kbTCj8kt1hvwC?key=-1sFumYT9362moS_9ulh_Q)

#### Monitor the pipeline

Monitor the pipeline progress and view the build logs to see if there are any issues that need to be addressed.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfIUeT_P2Qblfc4tDwXz8oRP5gwfiShvm0hRtP_2_15AxJ3BchhsxK-kjWd4t7qRrEpvao8cezjSUepwo9KbXcTe6GL0ZfYcoxlBbWUWZjzjyzLAzlYtSFiLpgF99Y_geldcu3t_3vIPyGmVGLmy8HaB-o?key=-1sFumYT9362moS_9ulh_Q)

#### Interact with a cluster from terminal

Retrieve the status of an Amazon Elastic Container Service for Kubernetes (EKS) cluster\


`aws eks describe-cluster --region <region-name> --name <cluster-name> --query cluster.status`

#### Update the kubeconfig file

The kubeconfig file is used to manage the communication between the Kubernetes command-line tool (kubectl) and the cluster. Use the below command to update the kubeconfig file

#### <kbd>aws eks --region \<region-name> update-kubeconfig --name \<cluster-name></kbd>

#### Retrieve data from the cluster

The following commands are used to retrieve information from a Kubernetes cluster, including lists of all nodes, pods, and services, as well as deployment data:\


`kubectl get nodes`

`kubectl get pods`

`kubectl get services`

`kubectl get deployments`

#### Expose the service

To make a service accessible from outside the cluster, you need to expose it using a LoadBalancer. In this example project, a service has already been deployed through Jenkins and made accessible to the outside world via LoadBalancer.

If your service is not yet accessible from the internet, you can make it accessible by changing the service type to LoadBalancer before deploying it to the cluster.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdRV5WRxJ42v0VFJJ0B2M0zVGIhszxRxiDS6SbxsuTqVfDDlW4qsLmejgBeHqCV6RlecDU0fYh_9ErzfhyLetLyT45UeUlpHD8zG2CqY-0JoyYMI2QlCDx7mjnj_Q_hOtaVqviiUGsYbMNGhqwPTmYwNJQ0?key=-1sFumYT9362moS_9ulh_Q)

#### Service type LoadBalancer

Change your service type to LoadBalancer to make it accessible from the internet. You should do this before deploying the service to the cluster.

#### Get the external IP

Run the “kubectl get svc” command to get the list of all services in the cluster and their details, including the external IP.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdP6ES6gYRfprOVj0DQzmu8cPcgdM1dY55zXToipcahwmMi2uRUgAAVD9W3miA-qzwmVTXXekulWJgb3sm0Oiyr_cPmV-q6ry3weE__FqG_Bl4waCihJI4jTHFm6D0yc4CKzlCwwtdUzmvamjSLq0pfAoA?key=-1sFumYT9362moS_9ulh_Q)

#### Example

To view your application from the internet, you need to open a web browser and enter the external IP address followed by the port number in the following format: \<External-IP>:\<PORT>. However, before you do this, you need to make sure that the port where your service is running is open and accessible.

#### Allow the required ports

To allow access to the app, you will need to configure the security group associated with the worker nodes in the EKS cluster to allow incoming traffic on the port used by the service.
