## Working with service accounts on GCP

An introductory article about creating a storage bucket on GCP using terraform with a service account.

### What is a Service account?

It is a special kind of account which belongs to your application instead of an individual end user. It is
used by applications to make authorized API calls. 

Eg: an application that uses Google Cloud Datastore for data persistence would use a service account to authenticate its calls to the Google Cloud Datastore API.

Another important point is that a service account is identified by its email address, which is unique to the account.


### Pre-requisite

 - Ensure you have google cloud sdk installed. For more information about installation check out  [ SDK Install.
](https://cloud.google.com/sdk/docs/install) 

- You have a GCP project.

### Creating a service account
There are multiple ways to create service account like using gcloud or using GCP console. Here we will use console for all activities.

- Under IAM section, select service account and fill the form. Click **create and continue** followed by **Done**.
 
![Screenshot 2021-12-27 at 12.13.38 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640609057106/ZEuB2QqDM.png)

- Once service account is created we need to create a JSON key.
Click on the service account name and go to **keys** tab.

- Select **add key option** under keys tab, once you click on create a JSON file will get downloaded on your device. This file will be used in the coming steps.

![Screenshot 2021-12-27 at 6.10.03 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640609194041/ss34gg8vL.png)

### Assign appropriate permission
Once we have created the service account for terraform we need to assign permissions to it.

- On IAM section, click on add.
- Type in your service account's email address and under **role** select **storage admin**.

![Screenshot 2021-12-27 at 5.53.28 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640608286763/lhPM0pFBT.png)

### Terraform code

- Main.tf

```
resource "google_storage_bucket" "bucket" {
  name          = "test-for-terraform"
  location      = "asia"
  force_destroy = true
  project = "YOUR-PROJECT-ID"
}
``` 
- Providers.tf

```
 terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.5.0"
    }
  }
}

provider "google" {
}
``` 

### Setup

- An easy way to provide service account credentials is by setting the GOOGLE_APPLICATION_CREDENTIALS environment variable.

```
export GOOGLE_APPLICATION_CREDENTIALS="[PATH]"
``` 
where path will be the location where the service account key is downloaded.

Example

```
export GOOGLE_APPLICATION_CREDENTIALS="/home/terraform-sa.json"                                                                                                                         
 ``` 
- terraform init

![Screenshot 2021-12-27 at 7.13.30 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640612759543/kW91pebeu.png)

- terraform plan 

![Screenshot 2021-12-27 at 7.16.28 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640612840333/Jts_lyqTC.png)

- terraform apply

![Screenshot 2021-12-27 at 7.22.55 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640613257754/M7eLdVfEp.png)

- Once the resource is created you can verify from **Activity** section on GCP console that bucket was created using service account which we created.


![Screenshot 2021-12-27 at 7.18.55 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640613328489/98V7bwtfu.png)
