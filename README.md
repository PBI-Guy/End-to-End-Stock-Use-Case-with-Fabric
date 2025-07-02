# Welcome to the End-to-End Stock Use Case developed in Microsoft Fabric

This GitHub repo contains all necessary items to create the End-to-End Stock Use Case in Microsoft Fabric. 

<img src="./Documentation/PNG/Slide4.PNG" width="500">

The full documentation can be found [here](./Documentation/Solution%20Overview/README.md).
<br>
Please follow the step-by-step guide [here](./Documentation/README.md).

## Prerequisites

To make sure you can use this GitHub repo, a few things are required.

### 1. You need a Fabric Capacity
To be able to use this GitHub repo, a Fabric Capacity is required. You can either use a F2+, a Fabric Trial, or Power BI Premium. To activate a trial, you can follow [this link.](https://learn.microsoft.com/en-us/fabric/fundamentals/fabric-trial)

### 2. You need to enable some Tenant settings
To make sure you can sync your workspace with GitHub, you need to enable following tenat settings:
* [Users can create Fabric items](https://learn.microsoft.com/en-us/fabric/admin/fabric-switch)
* [Users can synchronize workspace items with their Git repositories](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-get-started?tabs=github%2CAzure%2Ccommit-to-git)
* [Users can sync workspaces items with GitHub repositories](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-get-started?tabs=github%2CAzure%2Ccommit-to-git)
* Optional: If you wish to activate a Fabric Trial, you need to enable [Users can try Microsoft Fabric paid features](https://learn.microsoft.com/en-us/power-bi/fundamentals/service-self-service-signup-purchase-for-power-bi?tabs=free-sign-up)

### 3. You need a Workspace backed up by a Fabric Capacity with admin rights to connect your GitHub repo
Make sure your Workspace is assigned to a Capacity - Fabric, Premium or Trial.

<img src="./Documentation/PNG/01%20License%20Mode%20Fabric%20Capacity.png" width="500">

Further, make sure you have admin rights on the workspace.

### 4. You need a GitHub user
If you don't have a GitHub user already, please follow [this link](https://github.com/signup?source=login) to create one. In the following screen shots you'll see how to create one with a dummy account. 

Sign up using an email, providing a secure password, and a username

<img src="./Documentation/PNG/02%20Sign%20up%20to%20GitHub.png" width="500">

Next, verfiy your account with one option.

<img src="./Documentation/PNG/03%20Verify%20your%20account.png" width="500">

### Creating multiple users and workspaces automatically

Thanks to [https://github.com/JohnDev13](https://github.com/JohnDev13) who provided a Python Notebook which allows you to automatically create users and workspaces, and assign them to a trial capacity if desired. This makes it possible to use this repository for hands-on training sessions as well. You can find the Notebook [here](.//Documentation/Python%20Script/Create%20users%20and%20workspaces%20automatically.ipynb)

