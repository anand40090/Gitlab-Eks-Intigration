# Gitlab-Eks-Intigration
Gitlab Eks Intigration 



> GitLab is an Open Source code repository and collaborative software development platform for large DevOps and DevSecOps projects. GitLab is free for individuals. GitLab offers a location for online code storage and capabilities for issue tracking and CI/CD.

### Architecture Diagram 
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/2c54278b-79aa-414f-87f0-d0ef7a9461ae)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/452746de-3d91-4cf0-bdf9-52e5186163ba)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/08e3e3cd-33bd-4393-bbc9-67a44dd64cca)



### Prerequsites 
- Working Kubernetes cluster
- Local or cloud githlab account
- Eksctl, Kubectl, docker engine installed system

### High level steps 
- Create gitlab account in https://gitlab.com
- Create one project for k8s connection
- Setup connection between working K8s cluster and gitlab
- Create github registrey to upload docker image
- Create .gitlab-ci.yml in gitlab repository for deployment
- setup gitlab to use Kubernetes cluster
****************************************************************************************************************************************
# End to end lab 

- `Login to the gitlab aacout on https://gitlab.com`
  
- `Create new project for k8s connetion for REPO B from the above architecture diagram, make it public`
  
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/9acc5094-78dd-429b-98e7-9ad2fa6ebfd2)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/374e67c5-d50d-49f4-8b52-bb021a8542e3)

- `Create new project for k8s connetion for REPO A from the above architecture diagram, make it public.
  This repository will be used for project data`

  ![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/f7addffa-1a31-4696-9f7c-b2f71cb18aa9)

- `Connect working kubernetes cluster with gitlab`
  > To ocnnect kubernetes cluster with gitlab, you first install agent in your cluster



