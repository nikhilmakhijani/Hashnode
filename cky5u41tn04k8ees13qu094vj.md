## Google Cloud Build Pipeline

This article will give you a basic introduction of Cloud Build and a small practical of how it works.
## What is Cloud Build?
- Cloud Build is a service that executes your builds on Google Cloud Platform's infrastructure
- You can write a build config to provide instructions to Cloud Build on what tasks to perform.
- A build config file contains instructions for Cloud Build to perform tasks based on your specifications.

Build steps
- Build steps are analogous to commands in a script and provide you with the flexibility of executing 
   arbitrary instructions in your build.
- A build step specifies an action that you want Cloud Build to perform.
- For each build step, Cloud Build executes a docker container as an instance of docker run.
- You can include up to 100 build steps in your config file.
- Example of the build step.
   - Here we have defined steps in which we are calling hashicorp container and running terraform init, plan and apply commands.

```   
  steps:
  - name: hashicorp/terraform
    dir: deployments/
    args: ['init']
  - name: hashicorp/terraform
    dir: deployments/
    args: ['plan','-out=terraform.tfplan']
  - name: hashicorp/terraform
    dir: deployments/
    args: ['apply','-auto-approve']
``` 

### Hands-on
 Creating a cloud build trigger to deploy a storage bucket using terraform. Every time you push to your github repo, trigger will get executed.
 
![](https://media.giphy.com/media/JrYnKirK79252t3wDs/giphy.gif)

#### Requirements
   1) GCP Project
   2) Github or Bitbucket repo-  You can clone from [here](https://github.com/nikhilmakhijani/Cloudbuild-demo)

#### Steps
- Enable cloud build API in your GCP project. Once you enable the API, you will be able to see a cloud build service account in the IAM section.
 Naming convention of service account is YOUR_PROJECT_NUMBER@cloudbuild.gserviceaccount.com
- Cloud build will use this service account to create resources on GCP. So it needs to have storage admin permission as we are creating a storage bucket.
- Connect your github repo to cloud source repositories.
  - Open  [Source repo](https://source.cloud.google.com). 
  - Click on Add a repository and select connect to external repository.
  
![Screenshot 2022-01-08 at 5.43.21 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641644791341/-rQRKIGme.png)
- Under Cloud build section on portal, click on create a trigger and fill the form.
    - **Name** - Name of the trigger
    - **Event** - Push to a branch
    - **Source** 
        - Repository - Connected GIthub or bitbucket repo
        - Branch - select branch name
    - **Configuration** 
         - Type - Autodected or Cloud Build configuration file (yaml or json)
         - Location - Repository as the yaml is in the repo
    
![Screenshot 2022-01-08 at 5.48.59 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641644801761/157IZgUh6.png)

- Create the trigger. Now in main.tf change the bucket name and commit.
![](https://media.giphy.com/media/JmUlkDJMifYiXAri23/giphy.gif)

- Once you commit, cloud build trigger will get executed and you check live logs and see a GCS bucket got created.
![Screenshot 2022-01-08 at 6.04.06 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641645331688/mP1-5i-ik.png)

![](https://media.giphy.com/media/XBWrLAZricVcBwL8r4/giphy.gif)





   





