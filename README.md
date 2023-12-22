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

### Usefull Links 
1. https://docs.gitlab.com/ee/user/clusters/agent/install/index.html
2. https://github.com/mkaraminejad/cicd_pipeline/tree/main/1-K8S-CICD


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

- **Connect working kubernetes cluster with gitlab**
  
1. To connect existing working kubernetes cluster with gitlab, you first install agent in your cluster
   - In the repository, in the default branch, create an agent configuration file at the root
   - For time being keep the yaml file balnk
     ```
     “.gitlab/agents/k8s-connections/config.yaml
     ```

![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/3ce6552a-c3a4-46ee-9c8a-bbfce980e4a6)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/5adbf2e7-091e-4d23-8b74-fb9872ddd7b9)

**You must register an agent before you can install the agent in your cluster. To register an agent -**
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/dc96f4cc-2bf7-449d-b1e5-a88874fa3e92)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/2087de2d-c173-4d70-a647-b2f3a4a06449)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/f37c3b5a-1759-414f-b9f4-d6b9ed0aa687)

**Run below mentioned code on the Linus or windows system from where K8s cluster is accessible using Eks or Kubectl**
**For this helm package manger to be installed on the system**
```
helm repo add gitlab https://charts.gitlab.io
helm repo update
helm upgrade --install k8s-connection gitlab/gitlab-agent \
    --namespace gitlab-agent-k8s-connection \
    --create-namespace \
    --set image.tag=v16.8.0-rc3 \
    --set config.token=glagent-KcWsVoFxgEj57ziDykbLVX17i9Rtg8TYKB5LRKEtVrA__BG14Q \
    --set config.kasAddress=wss://kas.gitlab.com
```
> Output
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/88843d92-6b70-487c-87d9-f966c0823d2a)

> Now K8s cluster and gitlab Connection is created now
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/56e366d2-6959-42a8-92c7-c0d5c9a58108)

- `Now upload project data in the REPO A 'K8s-data'on gitlab`
  
**Clone repository in your system and dump you project data in the path and upload on gitlab**
  
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/71c5c0f1-2c62-455f-90ba-1ec6fb6817e0)

![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/9c6b05ba-0500-4560-acde-e50ce73cccf2)

**Push reposiroty to gitlab** 

![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/2ccc196d-120d-4003-9694-7b26b05062c7)

**Build docker image to be pushed on gitlab reposiroty**

![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/59a813ee-d363-42d9-820c-d668e9b57cdc)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/f9b0c1f9-3e25-4cb9-9b06-b498455e528e)

**Push docker image on gitlab registery manually**

![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/1df2b7e3-7180-4f74-a7fd-434c1e1d9404)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/bc0ec904-2af5-4c53-9abe-acae62dd0d43)
![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/befea12d-c8dc-4454-aed1-4ca96e57e120)

_______________________________________________________________________________________________________________________

### Gitlab CI-CD pipeline
- `Now create .gitlab-ci.yml in our repository. This file is indicate we ci/cd pipeline is there. Below code will create image and push to gotlab registry.`

```
stages:
    - build
build_image:
  image: docker
  stage: build
  services:
    - docker:dind
  variables:
    IMAGE_TAG: $CI_REGISTRY_IMAGE:$CI_COMMIT_REF_SLUG
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $IMAGE_TAG .
    - docker push $IMAGE_TAG

```
> Output

![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/c827aa97-6f98-4301-bef2-100ed2a1ad6d)

> check the built image in the registory by CI-CD pipeline

![image](https://github.com/anand40090/Gitlab-Eks-Intigration/assets/32446706/124443eb-fbd8-4972-ac0b-7b20b103538c)




















    





