# Chef



## What the hell is this Chef?


## What the hell is this Chef?
![Chef Giphy](https://media.giphy.com/media/10u6gt11vnm812/giphy.gif)

- Configuration management solution
- Uset to manage server configuration with automation

## Chef Components

- Cookbook
  - consists of Recipes
- Chef Supermarket
  - contains public cookbooks
- Node
  - has a chef-client installed
- Chef-Server
  - central component
- Chef Developer Kit
  - (Swiss army) knife


## How Chef works

- DevOps engineer writes code that
  - describes the state of a node
  - is deployed to the Chef-server
- Chef-server knows the expected state of each server
- Chef-client queries Chef-server for updates


# Ohai
- Application running on each node
- Supplies attribute infos specific to that node 
- Example
  - Operating System
  - Hard drive space
  - Linux kernel version 

## How we use chef ?

- Download the ChefDK
- Create a new "repository"
- Create folder for cookbooks
- Create cookbook (chef generate cookbook)


# Roles

- Run-list for role is usually 
  - the same for all servers at the same "layer" in the stack
  - different across layers.

# Environments
- Examples: Production, Staging, Development
- Just a "label" that can be assigned to nodes 

# Data Bags
- Global Source of attrinbutes that any recipe can call upon
- Data can be encrypted

# Chef + Testing

- Combine Chef, Vagrant and Vmware Workstation/ VirtualBox to test cookbooks on local machine


# Chef + Containers



# Links
- https://downloads.chef.io/chefdk


# Sources

https://www.slideshare.net/JoshPadnick/introduction-to-chef-automate-your-infrastructure-by-modeling-it-in-code
