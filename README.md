# ormae
Microservices Deployment Challenges:

Deployment of monolithic applications implies that you run several identical copies of a single, usually large application. This is mostly done by provisioning N servers, be it physical or virtual, and running the application’s M instances on each one.

If you are planning to deploy a microservices application, then you must be familiar with a variety of frameworks and languages these services are written in. This is also one of the biggest challenges since each one of these services has its specific deployment, resource requirements, scaling, and monitoring requirements. Add to it, deploying services has to be quick, reliable, and cost-effective

Microservices Deployment Strategy:

Service Instance per Container:
In this pattern, each service instance operates in its respective container, which is a virtualization mechanism at the operating system level. Some of the popular container technologies are Docker and Solaris Zones.

For using this pattern, you need to package your service as a filesystem image comprising the applications and libraries needed to execute the service, popularly known as a container image. Once the service is packaged as a container image, you then need to launch one or more containers and can run several containers on a physical or virtual host. To manage multiple containers many developers like to use cluster managers such as Kubernetes.

Benefits:

Like Service Instance per Virtual Machine, this pattern also works in isolation. It allows you to track how many resources are being used by each container. One of the biggest advantages over VMs is that containers are lightweight and very fast to build. Since there is no OS boot mechanism, containers can start quickly.
Challenges:

Despite rapidly maturing infrastructure, Service Instance per Container Pattern is still behind the VMs infrastructure and is not as secure as VMs since they share the kernel of the host OS.

Like VMs, you are responsible for all the heavy lifting of administering the container images. You also have to administer the container infrastructure and probably the VM infrastructure if you do not have a hosted container solution such as Amazon EC2 Container Service (ECS).

Also, since most of the containers are deployed on an infrastructure that is priced per VM, it results in extra deployment cost and over-provisioning of VMs to cater to an unexpected spike in the load.

Deployment Strategy for Staging and UAT :

We need to take latest code from GitHub
Need to integrate GitWebhook with Jenkins,
for automatic code Building 

Step2:

Have to Integrate Maven with Jenkins.

Step3:

For Storing artifacts  we will use ECR

 Step4:
need to build the image in Docker.

Jenkins Pipeline for Staging Deployment :

pipeline {
    agent any 
        
        stages {

            stage ('checkout crm')  {
                steps {
                    git branch: 'development', credentialsId: 'shashank-gitlab', url: '<>'
                    sh 'git log'                   
                }
            }
            
            stage ('removing privious code from server') {
                steps {
                    sshPublisher(publishers: [sshPublisherDesc(configName: '<>: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '<>/*', execTimeout: 1200000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }

            stage ('deploying code and updating docker container') {
                steps {
                   sshPublisher(publishers: [sshPublisherDesc(configName: '<ABC>', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker-compose -f /opt/rise/application/docker-compose.yml --compatibility up -d --build frontend2', execTimeout: 1200000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '<ABC>', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }
    }
}




Deployment Strategy for Production :

Step:1  
We need to Pull Latest Code from GitLab.
Step:2
 We need to write Docker files for Admin,Backend  and Frontend Panels.
 Step:3
We need Build the Docker Image:
docker build -t <Image name> .
Step:4 
We need to Push the Image to ECR(Elastic Container Registry)
Step5:
We need to Deploy Application Using EKS.
•	For deployment we need to Write Kubernetes Mainfest File. 

 For Deployment Check need to use Below Command:
 kubectl get deployment
For  Re deploy we need to Use Below Command:
      kubectl edit deployment manager
Then just change the tag name of image it will automatically deploy Our Production.


preparation for to finding logs:

To see the logs we will use Promotheus and Graphana for Visualization.
And also we can use Cloudwatch also

plan to add alerts and monitoring across all of the mentioned components:

For alerts we need to configure Cloudwatch alaram. 
 



