AWS Services Used
EKS (Elastic Kubernetes Service): It is a managed kubernetes service by AWS, which makes it easy to deploy and scale containerised applications on kubernetes. Basically master nodes are managed by aws which are distributed across multiple availability zones, which makes it highly available.
ECR (Elastic Container Registry): It is a docker registry service managed by AWS, I have used it to push docker images after build, further these images pulled and deployed on kubernetes cluster.
Pipeline Steps
Developer pushes the code, since it is branch agnostic, developer can push to any branch.
A webhook in github or in gitlab will trigger a jenkins build.
Jenkins will pull the source code and has series of steps
Build: Create a docker image with a tag, this tag will be jenkins BUILD_NUMBER
Push Image: This docker image is pushed to the ECR repository.
Integration Test: In this test we deploy the image on a different namespace and do the testing on that namespace.
If testing is successful it will deploy to release namespace else abort the deployment
Rollback Procedure
Rollback pretty simple using helm, there is a script called rollback.sh, which will display the history of deployments, where we can decide which deployment version needs to be rollback.

Rollback

This is how rollback will look like when you execute ci/rollback.sh, it will show you the state of current deployment and after deployment. As it can be seen before rolling back docker image tag  and after rolling back docker image tag .

Prerequisites
Docker
Kubernetes
awscli
helm
terraform
Setting up Infrastructure
Go to terraform directory, execute below command

terraform plan

then execute

terraform apply

this will prompt with yes or no
